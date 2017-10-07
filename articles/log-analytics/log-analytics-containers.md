---
title: "aaaContainer monitorování řešení v Azure Log Analytics | Microsoft Docs"
description: "Hello monitorování kontejneru řešení v analýzy protokolů umožňuje zobrazení a správa Docker a Windows hostitele kontejneru na jednom místě."
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
ms.openlocfilehash: 2eed1dd81c22faef78a375fca3ebece9e5300c09
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="container-monitoring-solution-in-log-analytics"></a><span data-ttu-id="34f3d-103">Řešení monitorování kontejneru v analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="34f3d-103">Container Monitoring solution in Log Analytics</span></span>

![Kontejnery – symbol](./media/log-analytics-containers/containers-symbol.png)

<span data-ttu-id="34f3d-105">Tento článek popisuje, jak tooset nahoru a používání hello monitorování kontejneru řešení v analýzy protokolů, která slouží k zobrazení a správa Docker a Windows hostitele kontejneru na jednom místě.</span><span class="sxs-lookup"><span data-stu-id="34f3d-105">This article describes how tooset up and use hello Container Monitoring solution in Log Analytics, which helps you view and manage your Docker and Windows container hosts in a single location.</span></span> <span data-ttu-id="34f3d-106">Docker je použit systém virtualizační software toocreate kontejnery, které automatizují tootheir nasazení softwaru IT infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="34f3d-106">Docker is a software virtualization system used toocreate containers that automate software deployment tootheir IT infrastructure.</span></span>

<span data-ttu-id="34f3d-107">Hello řešení zobrazuje, který kontejnery běží, jaké kontejneru bitové kopie, že používáte systém, a kde kontejnery běží.</span><span class="sxs-lookup"><span data-stu-id="34f3d-107">hello solution shows which containers are running, what container image they’re running, and where containers are running.</span></span> <span data-ttu-id="34f3d-108">Můžete zobrazit podrobné informace o auditování zobrazující příkazy používané s kontejnery.</span><span class="sxs-lookup"><span data-stu-id="34f3d-108">You can view detailed audit information showing commands used with containers.</span></span> <span data-ttu-id="34f3d-109">A kontejnery můžete vyřešit tak, že zobrazení a hledání centralizované protokoly bez nutnosti tooremotely zobrazení Docker nebo hostitelů systému Windows.</span><span class="sxs-lookup"><span data-stu-id="34f3d-109">And, you can troubleshoot containers by viewing and searching centralized logs without having tooremotely view Docker or Windows hosts.</span></span> <span data-ttu-id="34f3d-110">Můžete najít kontejnery, které může být aktivní nebo využívání nadbytečné prostředky na hostiteli.</span><span class="sxs-lookup"><span data-stu-id="34f3d-110">You can find containers that may be noisy and consuming excess resources on a host.</span></span> <span data-ttu-id="34f3d-111">A lze je zobrazit centralizované procesoru, paměti, úložiště a využití a výkonu informace o síti pro kontejnery.</span><span class="sxs-lookup"><span data-stu-id="34f3d-111">And, you can view centralized CPU, memory, storage, and network usage and performance information for containers.</span></span> <span data-ttu-id="34f3d-112">V počítačích se systémem Windows, můžete centralizovat a porovnání protokoly ze systému Windows Server, technologie Hyper-V a Docker kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="34f3d-112">On computers running Windows, you can centralize and compare logs from Windows Server, Hyper-V, and Docker containers.</span></span> <span data-ttu-id="34f3d-113">řešení Hello podporuje hello následující orchestrators kontejneru:</span><span class="sxs-lookup"><span data-stu-id="34f3d-113">hello solution supports hello following container orchestrators:</span></span>

- <span data-ttu-id="34f3d-114">Docker Swarm</span><span class="sxs-lookup"><span data-stu-id="34f3d-114">Docker Swarm</span></span>
- <span data-ttu-id="34f3d-115">DC/OS</span><span class="sxs-lookup"><span data-stu-id="34f3d-115">DC/OS</span></span>
- <span data-ttu-id="34f3d-116">Kubernetes</span><span class="sxs-lookup"><span data-stu-id="34f3d-116">Kubernetes</span></span>
- <span data-ttu-id="34f3d-117">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="34f3d-117">Service Fabric</span></span>
- <span data-ttu-id="34f3d-118">Red Hat OpenShift</span><span class="sxs-lookup"><span data-stu-id="34f3d-118">Red Hat OpenShift</span></span>


<span data-ttu-id="34f3d-119">Hello následující diagram znázorňuje hello vztahy mezi různými hostiteli kontejneru a agentů v OMS.</span><span class="sxs-lookup"><span data-stu-id="34f3d-119">hello following diagram shows hello relationships between various container hosts and agents with OMS.</span></span>

![Diagram kontejnery](./media/log-analytics-containers/containers-diagram.png)

## <a name="system-requirements"></a><span data-ttu-id="34f3d-121">Požadavky na systém</span><span class="sxs-lookup"><span data-stu-id="34f3d-121">System requirements</span></span>

<span data-ttu-id="34f3d-122">Než začnete, zkontrolujte následující podrobnosti tooverify splňujete požadavky hello hello.</span><span class="sxs-lookup"><span data-stu-id="34f3d-122">Before starting, review hello following details tooverify you meet hello prerequisites.</span></span>

### <a name="container-monitoring-solution-support-for-docker-orchestrator-and-os-platform"></a><span data-ttu-id="34f3d-123">Podpora řešení monitorování kontejneru pro platformy Docker Orchestrator a verze operačního systému</span><span class="sxs-lookup"><span data-stu-id="34f3d-123">Container monitoring solution support for Docker Orchestrator and OS platform</span></span>
<span data-ttu-id="34f3d-124">Hello následující tabulka obsahuje přehled hello Docker orchestration a operační systém monitorování podporu kontejneru inventáře, výkonu a protokoly s analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="34f3d-124">hello following table outlines hello Docker orchestration and operating system monitoring support of container inventory, performance, and logs with Log Analytics.</span></span>   

| | <span data-ttu-id="34f3d-125">ACS</span><span class="sxs-lookup"><span data-stu-id="34f3d-125">ACS</span></span> | <span data-ttu-id="34f3d-126">Linux</span><span class="sxs-lookup"><span data-stu-id="34f3d-126">Linux</span></span> | <span data-ttu-id="34f3d-127">Windows</span><span class="sxs-lookup"><span data-stu-id="34f3d-127">Windows</span></span> | <span data-ttu-id="34f3d-128">Kontejner</span><span class="sxs-lookup"><span data-stu-id="34f3d-128">Container</span></span><br><span data-ttu-id="34f3d-129">Inventáře</span><span class="sxs-lookup"><span data-stu-id="34f3d-129">Inventory</span></span> | <span data-ttu-id="34f3d-130">Image</span><span class="sxs-lookup"><span data-stu-id="34f3d-130">Image</span></span><br><span data-ttu-id="34f3d-131">Inventáře</span><span class="sxs-lookup"><span data-stu-id="34f3d-131">Inventory</span></span> | <span data-ttu-id="34f3d-132">Node</span><span class="sxs-lookup"><span data-stu-id="34f3d-132">Node</span></span><br><span data-ttu-id="34f3d-133">Inventáře</span><span class="sxs-lookup"><span data-stu-id="34f3d-133">Inventory</span></span> | <span data-ttu-id="34f3d-134">Kontejner</span><span class="sxs-lookup"><span data-stu-id="34f3d-134">Container</span></span><br><span data-ttu-id="34f3d-135">Výkon</span><span class="sxs-lookup"><span data-stu-id="34f3d-135">Performance</span></span> | <span data-ttu-id="34f3d-136">Kontejner</span><span class="sxs-lookup"><span data-stu-id="34f3d-136">Container</span></span><br><span data-ttu-id="34f3d-137">Událost</span><span class="sxs-lookup"><span data-stu-id="34f3d-137">Event</span></span> | <span data-ttu-id="34f3d-138">Událost</span><span class="sxs-lookup"><span data-stu-id="34f3d-138">Event</span></span><br><span data-ttu-id="34f3d-139">Protokol</span><span class="sxs-lookup"><span data-stu-id="34f3d-139">Log</span></span> | <span data-ttu-id="34f3d-140">Kontejner</span><span class="sxs-lookup"><span data-stu-id="34f3d-140">Container</span></span><br><span data-ttu-id="34f3d-141">Protokol</span><span class="sxs-lookup"><span data-stu-id="34f3d-141">Log</span></span> |
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
| <span data-ttu-id="34f3d-142">Kubernetes</span><span class="sxs-lookup"><span data-stu-id="34f3d-142">Kubernetes</span></span> | <span data-ttu-id="34f3d-143">&#8226;</span><span class="sxs-lookup"><span data-stu-id="34f3d-143">&#8226;</span></span> | <span data-ttu-id="34f3d-144">&#8226;</span><span class="sxs-lookup"><span data-stu-id="34f3d-144">&#8226;</span></span> | | <span data-ttu-id="34f3d-145">&#8226;</span><span class="sxs-lookup"><span data-stu-id="34f3d-145">&#8226;</span></span> | <span data-ttu-id="34f3d-146">&#8226;</span><span class="sxs-lookup"><span data-stu-id="34f3d-146">&#8226;</span></span> | <span data-ttu-id="34f3d-147">&#8226;</span><span class="sxs-lookup"><span data-stu-id="34f3d-147">&#8226;</span></span> | <span data-ttu-id="34f3d-148">&#8226;</span><span class="sxs-lookup"><span data-stu-id="34f3d-148">&#8226;</span></span> | <span data-ttu-id="34f3d-149">&#8226;</span><span class="sxs-lookup"><span data-stu-id="34f3d-149">&#8226;</span></span> | <span data-ttu-id="34f3d-150">&#8226;</span><span class="sxs-lookup"><span data-stu-id="34f3d-150">&#8226;</span></span> | <span data-ttu-id="34f3d-151">&#8226;</span><span class="sxs-lookup"><span data-stu-id="34f3d-151">&#8226;</span></span> |
| <span data-ttu-id="34f3d-152">Mesosphere</span><span class="sxs-lookup"><span data-stu-id="34f3d-152">Mesosphere</span></span><br><span data-ttu-id="34f3d-153">DC/OS</span><span class="sxs-lookup"><span data-stu-id="34f3d-153">DC/OS</span></span> | <span data-ttu-id="34f3d-154">&#8226;</span><span class="sxs-lookup"><span data-stu-id="34f3d-154">&#8226;</span></span> | <span data-ttu-id="34f3d-155">&#8226;</span><span class="sxs-lookup"><span data-stu-id="34f3d-155">&#8226;</span></span> | | <span data-ttu-id="34f3d-156">&#8226;</span><span class="sxs-lookup"><span data-stu-id="34f3d-156">&#8226;</span></span> | <span data-ttu-id="34f3d-157">&#8226;</span><span class="sxs-lookup"><span data-stu-id="34f3d-157">&#8226;</span></span> | <span data-ttu-id="34f3d-158">&#8226;</span><span class="sxs-lookup"><span data-stu-id="34f3d-158">&#8226;</span></span> | <span data-ttu-id="34f3d-159">&#8226;</span><span class="sxs-lookup"><span data-stu-id="34f3d-159">&#8226;</span></span>| <span data-ttu-id="34f3d-160">&#8226;</span><span class="sxs-lookup"><span data-stu-id="34f3d-160">&#8226;</span></span> | <span data-ttu-id="34f3d-161">&#8226;</span><span class="sxs-lookup"><span data-stu-id="34f3d-161">&#8226;</span></span> | <span data-ttu-id="34f3d-162">&#8226;</span><span class="sxs-lookup"><span data-stu-id="34f3d-162">&#8226;</span></span> |
| <span data-ttu-id="34f3d-163">Docker</span><span class="sxs-lookup"><span data-stu-id="34f3d-163">Docker</span></span><br><span data-ttu-id="34f3d-164">Swarm</span><span class="sxs-lookup"><span data-stu-id="34f3d-164">Swarm</span></span> | <span data-ttu-id="34f3d-165">&#8226;</span><span class="sxs-lookup"><span data-stu-id="34f3d-165">&#8226;</span></span> | <span data-ttu-id="34f3d-166">&#8226;</span><span class="sxs-lookup"><span data-stu-id="34f3d-166">&#8226;</span></span> | <span data-ttu-id="34f3d-167">&#8226;</span><span class="sxs-lookup"><span data-stu-id="34f3d-167">&#8226;</span></span> | <span data-ttu-id="34f3d-168">&#8226;</span><span class="sxs-lookup"><span data-stu-id="34f3d-168">&#8226;</span></span> | <span data-ttu-id="34f3d-169">&#8226;</span><span class="sxs-lookup"><span data-stu-id="34f3d-169">&#8226;</span></span> | <span data-ttu-id="34f3d-170">&#8226;</span><span class="sxs-lookup"><span data-stu-id="34f3d-170">&#8226;</span></span> | <span data-ttu-id="34f3d-171">&#8226;</span><span class="sxs-lookup"><span data-stu-id="34f3d-171">&#8226;</span></span> | <span data-ttu-id="34f3d-172">&#8226;</span><span class="sxs-lookup"><span data-stu-id="34f3d-172">&#8226;</span></span> | | <span data-ttu-id="34f3d-173">&#8226;</span><span class="sxs-lookup"><span data-stu-id="34f3d-173">&#8226;</span></span> |
| <span data-ttu-id="34f3d-174">Služba</span><span class="sxs-lookup"><span data-stu-id="34f3d-174">Service</span></span><br><span data-ttu-id="34f3d-175">Prostředky infrastruktury</span><span class="sxs-lookup"><span data-stu-id="34f3d-175">Fabric</span></span> | | | <span data-ttu-id="34f3d-176">&#8226;</span><span class="sxs-lookup"><span data-stu-id="34f3d-176">&#8226;</span></span> | <span data-ttu-id="34f3d-177">&#8226;</span><span class="sxs-lookup"><span data-stu-id="34f3d-177">&#8226;</span></span> | <span data-ttu-id="34f3d-178">&#8226;</span><span class="sxs-lookup"><span data-stu-id="34f3d-178">&#8226;</span></span> | <span data-ttu-id="34f3d-179">&#8226;</span><span class="sxs-lookup"><span data-stu-id="34f3d-179">&#8226;</span></span> | <span data-ttu-id="34f3d-180">&#8226;</span><span class="sxs-lookup"><span data-stu-id="34f3d-180">&#8226;</span></span> | <span data-ttu-id="34f3d-181">&#8226;</span><span class="sxs-lookup"><span data-stu-id="34f3d-181">&#8226;</span></span> | <span data-ttu-id="34f3d-182">&#8226;</span><span class="sxs-lookup"><span data-stu-id="34f3d-182">&#8226;</span></span> | <span data-ttu-id="34f3d-183">&#8226;</span><span class="sxs-lookup"><span data-stu-id="34f3d-183">&#8226;</span></span> |
| <span data-ttu-id="34f3d-184">Otevřete Red Hat</span><span class="sxs-lookup"><span data-stu-id="34f3d-184">Red Hat Open</span></span><br><span data-ttu-id="34f3d-185">Posunutí</span><span class="sxs-lookup"><span data-stu-id="34f3d-185">Shift</span></span> | | <span data-ttu-id="34f3d-186">&#8226;</span><span class="sxs-lookup"><span data-stu-id="34f3d-186">&#8226;</span></span> | | <span data-ttu-id="34f3d-187">&#8226;</span><span class="sxs-lookup"><span data-stu-id="34f3d-187">&#8226;</span></span> | <span data-ttu-id="34f3d-188">&#8226;</span><span class="sxs-lookup"><span data-stu-id="34f3d-188">&#8226;</span></span>| <span data-ttu-id="34f3d-189">&#8226;</span><span class="sxs-lookup"><span data-stu-id="34f3d-189">&#8226;</span></span> | <span data-ttu-id="34f3d-190">&#8226;</span><span class="sxs-lookup"><span data-stu-id="34f3d-190">&#8226;</span></span> | <span data-ttu-id="34f3d-191">&#8226;</span><span class="sxs-lookup"><span data-stu-id="34f3d-191">&#8226;</span></span> | | <span data-ttu-id="34f3d-192">&#8226;</span><span class="sxs-lookup"><span data-stu-id="34f3d-192">&#8226;</span></span> |
| <span data-ttu-id="34f3d-193">Windows Server</span><span class="sxs-lookup"><span data-stu-id="34f3d-193">Windows Server</span></span><br><span data-ttu-id="34f3d-194">(samostatně)</span><span class="sxs-lookup"><span data-stu-id="34f3d-194">(standalone)</span></span> | | | <span data-ttu-id="34f3d-195">&#8226;</span><span class="sxs-lookup"><span data-stu-id="34f3d-195">&#8226;</span></span> | <span data-ttu-id="34f3d-196">&#8226;</span><span class="sxs-lookup"><span data-stu-id="34f3d-196">&#8226;</span></span> | <span data-ttu-id="34f3d-197">&#8226;</span><span class="sxs-lookup"><span data-stu-id="34f3d-197">&#8226;</span></span> | <span data-ttu-id="34f3d-198">&#8226;</span><span class="sxs-lookup"><span data-stu-id="34f3d-198">&#8226;</span></span> | <span data-ttu-id="34f3d-199">&#8226;</span><span class="sxs-lookup"><span data-stu-id="34f3d-199">&#8226;</span></span> | <span data-ttu-id="34f3d-200">&#8226;</span><span class="sxs-lookup"><span data-stu-id="34f3d-200">&#8226;</span></span> | | <span data-ttu-id="34f3d-201">&#8226;</span><span class="sxs-lookup"><span data-stu-id="34f3d-201">&#8226;</span></span> |
| <span data-ttu-id="34f3d-202">Linux Server</span><span class="sxs-lookup"><span data-stu-id="34f3d-202">Linux Server</span></span><br><span data-ttu-id="34f3d-203">(samostatně)</span><span class="sxs-lookup"><span data-stu-id="34f3d-203">(standalone)</span></span> | | <span data-ttu-id="34f3d-204">&#8226;</span><span class="sxs-lookup"><span data-stu-id="34f3d-204">&#8226;</span></span> | | <span data-ttu-id="34f3d-205">&#8226;</span><span class="sxs-lookup"><span data-stu-id="34f3d-205">&#8226;</span></span> | <span data-ttu-id="34f3d-206">&#8226;</span><span class="sxs-lookup"><span data-stu-id="34f3d-206">&#8226;</span></span> | <span data-ttu-id="34f3d-207">&#8226;</span><span class="sxs-lookup"><span data-stu-id="34f3d-207">&#8226;</span></span> | <span data-ttu-id="34f3d-208">&#8226;</span><span class="sxs-lookup"><span data-stu-id="34f3d-208">&#8226;</span></span> | <span data-ttu-id="34f3d-209">&#8226;</span><span class="sxs-lookup"><span data-stu-id="34f3d-209">&#8226;</span></span> | | <span data-ttu-id="34f3d-210">&#8226;</span><span class="sxs-lookup"><span data-stu-id="34f3d-210">&#8226;</span></span> |


### <a name="docker-versions-supported-on-linux"></a><span data-ttu-id="34f3d-211">Verze docker podporované v systému Linux</span><span class="sxs-lookup"><span data-stu-id="34f3d-211">Docker versions supported on Linux</span></span>

- <span data-ttu-id="34f3d-212">Docker 1.11 too1.13</span><span class="sxs-lookup"><span data-stu-id="34f3d-212">Docker 1.11 too1.13</span></span>
- <span data-ttu-id="34f3d-213">V17.06 docker CE a EE</span><span class="sxs-lookup"><span data-stu-id="34f3d-213">Docker CE and EE v17.06</span></span>

### <a name="x64-linux-distributions-supported-as-container-hosts"></a><span data-ttu-id="34f3d-214">x64 Linuxových distribucích podporovány jako hostitele kontejneru</span><span class="sxs-lookup"><span data-stu-id="34f3d-214">x64 Linux distributions supported as container hosts</span></span>

- <span data-ttu-id="34f3d-215">Ubuntu 14.04 LTS a 16.04 LTS</span><span class="sxs-lookup"><span data-stu-id="34f3d-215">Ubuntu 14.04 LTS and 16.04 LTS</span></span>
- <span data-ttu-id="34f3d-216">CoreOS(stable)</span><span class="sxs-lookup"><span data-stu-id="34f3d-216">CoreOS(stable)</span></span>
- <span data-ttu-id="34f3d-217">Amazon Linux 2016.09.0</span><span class="sxs-lookup"><span data-stu-id="34f3d-217">Amazon Linux 2016.09.0</span></span>
- <span data-ttu-id="34f3d-218">openSUSE 13.2</span><span class="sxs-lookup"><span data-stu-id="34f3d-218">openSUSE 13.2</span></span>
- <span data-ttu-id="34f3d-219">openSUSE LEAP 42.2</span><span class="sxs-lookup"><span data-stu-id="34f3d-219">openSUSE LEAP 42.2</span></span>
- <span data-ttu-id="34f3d-220">CentOS 7.2 a 7.3</span><span class="sxs-lookup"><span data-stu-id="34f3d-220">CentOS 7.2 and 7.3</span></span>
- <span data-ttu-id="34f3d-221">SLES 12</span><span class="sxs-lookup"><span data-stu-id="34f3d-221">SLES 12</span></span>
- <span data-ttu-id="34f3d-222">RHEL 7.2 a 7.3</span><span class="sxs-lookup"><span data-stu-id="34f3d-222">RHEL 7.2 and 7.3</span></span>
- <span data-ttu-id="34f3d-223">Red Hat OpenShift kontejneru platformy (OCP) 3.4 a 3.5</span><span class="sxs-lookup"><span data-stu-id="34f3d-223">Red Hat OpenShift Container Platform (OCP) 3.4 and 3.5</span></span>
- <span data-ttu-id="34f3d-224">Too1.8.8 DC/OS 1.7.3 Mesosphere služby ACS</span><span class="sxs-lookup"><span data-stu-id="34f3d-224">ACS Mesosphere DC/OS 1.7.3 too1.8.8</span></span>
- <span data-ttu-id="34f3d-225">Služba ACS Kubernetes 1.4.5 too1.6</span><span class="sxs-lookup"><span data-stu-id="34f3d-225">ACS Kubernetes 1.4.5 too1.6</span></span>
- <span data-ttu-id="34f3d-226">Služba ACS Docker Swarm</span><span class="sxs-lookup"><span data-stu-id="34f3d-226">ACS Docker Swarm</span></span>

### <a name="supported-windows-operating-system"></a><span data-ttu-id="34f3d-227">Podporovaný operační systém Windows</span><span class="sxs-lookup"><span data-stu-id="34f3d-227">Supported Windows operating system</span></span>

- <span data-ttu-id="34f3d-228">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="34f3d-228">Windows Server 2016</span></span>
- <span data-ttu-id="34f3d-229">Windows 10 Anniversary Edition (Professional nebo Enterprise)</span><span class="sxs-lookup"><span data-stu-id="34f3d-229">Windows 10 Anniversary Edition (Professional or Enterprise)</span></span>

### <a name="docker-versions-supported-on-windows"></a><span data-ttu-id="34f3d-230">Verze docker podporované v systému Windows</span><span class="sxs-lookup"><span data-stu-id="34f3d-230">Docker versions supported on Windows</span></span>

- <span data-ttu-id="34f3d-231">Docker 1.12 a 1.13</span><span class="sxs-lookup"><span data-stu-id="34f3d-231">Docker 1.12 and 1.13</span></span>
- <span data-ttu-id="34f3d-232">Docker 17.03.0 a novější</span><span class="sxs-lookup"><span data-stu-id="34f3d-232">Docker 17.03.0 and later</span></span>

## <a name="installing-and-configuring-hello-solution"></a><span data-ttu-id="34f3d-233">Instalace a konfigurace řešení hello</span><span class="sxs-lookup"><span data-stu-id="34f3d-233">Installing and configuring hello solution</span></span>
<span data-ttu-id="34f3d-234">Použijte následující informace tooinstall hello a nakonfigurujte hello řešení.</span><span class="sxs-lookup"><span data-stu-id="34f3d-234">Use hello following information tooinstall and configure hello solution.</span></span>

1. <span data-ttu-id="34f3d-235">Přidat hello monitorování kontejneru řešení tooyour pracovním prostorem OMS z [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.ContainersOMS?tab=Overview) nebo pomocí hello procesu popsaného v tématu [řešení přidat analýzy protokolů z hello řešení Galerie](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="34f3d-235">Add hello Container Monitoring solution tooyour OMS workspace from [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.ContainersOMS?tab=Overview) or by using hello process described in [Add Log Analytics solutions from hello Solutions Gallery](log-analytics-add-solutions.md).</span></span>

2. <span data-ttu-id="34f3d-236">Nainstalovat a používat Docker s agentem OMS.</span><span class="sxs-lookup"><span data-stu-id="34f3d-236">Install and use Docker with an OMS agent.</span></span>  <span data-ttu-id="34f3d-237">V závislosti na použitém operačním systému, můžete vybrat z hello následující metody:</span><span class="sxs-lookup"><span data-stu-id="34f3d-237">Based on your operating system, you can choose from hello following methods:</span></span>

  * <span data-ttu-id="34f3d-238">Na podporovaných operačních systémů Linux, nainstalujte a spusťte Docker a poté nainstalujte a nakonfigurujte hello [OMS agenta pro Linux](log-analytics-agent-linux.md).</span><span class="sxs-lookup"><span data-stu-id="34f3d-238">On supported Linux operating systems, install and run Docker and then install and configure hello [OMS Agent for Linux](log-analytics-agent-linux.md).</span></span>  
  * <span data-ttu-id="34f3d-239">Na jádro operačního systému nelze spustit hello OMS agenta pro Linux.</span><span class="sxs-lookup"><span data-stu-id="34f3d-239">On CoreOS, you cannot run hello OMS Agent for Linux.</span></span> <span data-ttu-id="34f3d-240">Místo toho spustíte kontejnerizované verzi hello OMS agenta pro Linux.</span><span class="sxs-lookup"><span data-stu-id="34f3d-240">Instead, you run a containerized version of hello OMS Agent for Linux.</span></span> <span data-ttu-id="34f3d-241">Zkontrolujte [Linux kontejneru hostitele, včetně CoreOS](#for-all-linux-container-hosts-including-coreos) nebo [hostitele kontejner Azure Government Linux včetně CoreOS](#for-all-azure-government-linux-container-hosts-including-coreos) při práci s kontejnery v cloudu Azure Government.</span><span class="sxs-lookup"><span data-stu-id="34f3d-241">Review [Linux container hosts including CoreOS](#for-all-linux-container-hosts-including-coreos) or [Azure Government Linux container hosts including CoreOS](#for-all-azure-government-linux-container-hosts-including-coreos) if you are working with containers in Azure Government Cloud.</span></span>
  * <span data-ttu-id="34f3d-242">V systému Windows Server 2016 a Windows 10 instalaci hello modulu Docker a klienta pak připojit agenta toogather informace a odeslat tooLog Analytics.</span><span class="sxs-lookup"><span data-stu-id="34f3d-242">On Windows Server 2016 and Windows 10, install hello Docker Engine and client then connect an agent toogather information and send it tooLog Analytics.</span></span>  

### <a name="container-services"></a><span data-ttu-id="34f3d-243">Služby kontejneru</span><span class="sxs-lookup"><span data-stu-id="34f3d-243">Container services</span></span>

- <span data-ttu-id="34f3d-244">Pokud máte Red Hat OpenShift prostředí, přečtěte si [konfigurace agenta OMS pro Red Hat OpenShift](#configure-an-oms-agent-for-red-hat-openshift).</span><span class="sxs-lookup"><span data-stu-id="34f3d-244">If you have a Red Hat OpenShift environment, review [Configure an OMS agent for Red Hat OpenShift](#configure-an-oms-agent-for-red-hat-openshift).</span></span>
- <span data-ttu-id="34f3d-245">Pokud máte cluster Kubernetes pomocí hello Azure Container Service, přečtěte si [monitorování clusteru Azure Container Service s Operations Management Suite (OMS)](../container-service/kubernetes/container-service-kubernetes-oms.md).</span><span class="sxs-lookup"><span data-stu-id="34f3d-245">If you have a Kubernetes cluster using hello Azure Container Service, review [Monitor an Azure Container Service cluster with Microsoft Operations Management Suite (OMS)](../container-service/kubernetes/container-service-kubernetes-oms.md).</span></span>
- <span data-ttu-id="34f3d-246">Pokud máte cluster Azure Container Service DC/OS, přečtěte si informace v [monitorování clusteru Azure Container Service DC/OS s Operations Management Suite](../container-service/dcos-swarm/container-service-monitoring-oms.md).</span><span class="sxs-lookup"><span data-stu-id="34f3d-246">If you have an Azure Container Service DC/OS cluster, learn more at [Monitor an Azure Container Service DC/OS cluster with Operations Management Suite](../container-service/dcos-swarm/container-service-monitoring-oms.md).</span></span>
- <span data-ttu-id="34f3d-247">Pokud máte prostředí režimu Docker Swarm, další informace v [konfigurace agenta OMS pro Docker Swarm](#configure-an-oms-agent-for-docker-swarm).</span><span class="sxs-lookup"><span data-stu-id="34f3d-247">If you have a Docker Swarm mode environment, learn more at [Configure an OMS agent for Docker Swarm](#configure-an-oms-agent-for-docker-swarm).</span></span>
- <span data-ttu-id="34f3d-248">Pokud používáte kontejnery s Service Fabric, další informace v [přehled Azure Service Fabric](../service-fabric/service-fabric-overview.md).</span><span class="sxs-lookup"><span data-stu-id="34f3d-248">If you use containers with Service Fabric, learn more at [Overview of Azure Service Fabric](../service-fabric/service-fabric-overview.md).</span></span>
- <span data-ttu-id="34f3d-249">Zkontrolujte hello [modulu Docker v systému Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon) Další informace o tom, najdete v článku tooinstall a konfigurace vašeho Docker moduly v počítačích se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="34f3d-249">Review hello [Docker Engine on Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon) article for additional information about how tooinstall and configure your Docker Engines on computers running Windows.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="34f3d-250">Docker musí být spuštěna **před** nainstalujete hello [OMS agenta pro Linux](log-analytics-agent-linux.md) v hostitelích kontejneru.</span><span class="sxs-lookup"><span data-stu-id="34f3d-250">Docker must be running **before** you install hello [OMS Agent for Linux](log-analytics-agent-linux.md) on your container hosts.</span></span> <span data-ttu-id="34f3d-251">Pokud jste již nainstalovali agenta hello před instalací Docker, musíte tooreinstall hello OMS agenta pro Linux.</span><span class="sxs-lookup"><span data-stu-id="34f3d-251">If you've already installed hello agent before installing Docker, you need tooreinstall hello OMS Agent for Linux.</span></span> <span data-ttu-id="34f3d-252">Další informace o Docker najdete v tématu hello [Docker webu](https://www.docker.com).</span><span class="sxs-lookup"><span data-stu-id="34f3d-252">For more information about Docker, see hello [Docker website](https://www.docker.com).</span></span>


## <a name="linux-container-hosts"></a><span data-ttu-id="34f3d-253">Linux kontejneru hostitele</span><span class="sxs-lookup"><span data-stu-id="34f3d-253">Linux container hosts</span></span>

<span data-ttu-id="34f3d-254">Po instalaci Docker, použijte následující nastavení pro agenta kontejneru hostitele tooconfigure hello pro použití s nástrojem Docker hello.</span><span class="sxs-lookup"><span data-stu-id="34f3d-254">After you've installed Docker, use hello following settings for your container host tooconfigure hello agent for use with Docker.</span></span> <span data-ttu-id="34f3d-255">Je třeba nejprve vaše OMS ID a klíč, který můžete najít v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="34f3d-255">First you need your OMS workspace ID and key, which you can find in hello Azure portal.</span></span> <span data-ttu-id="34f3d-256">V pracovním prostoru, klikněte na tlačítko **rychlý Start** > **počítače** tooview vaše **ID pracovního prostoru** a **primární klíč**.</span><span class="sxs-lookup"><span data-stu-id="34f3d-256">In your workspace, click **Quick Start** > **Computers** tooview your **Workspace ID** and **Primary Key**.</span></span>  <span data-ttu-id="34f3d-257">Zkopírujte a vložte do vašeho oblíbeného editoru.</span><span class="sxs-lookup"><span data-stu-id="34f3d-257">Copy and paste both into your favorite editor.</span></span>

### <a name="for-all-linux-container-hosts-except-coreos"></a><span data-ttu-id="34f3d-258">Pro všechny hostitele kontejneru Linux s výjimkou CoreOS</span><span class="sxs-lookup"><span data-stu-id="34f3d-258">For all Linux container hosts except CoreOS</span></span>

- <span data-ttu-id="34f3d-259">Další informace a kroky na tom, jak tooinstall hello OMS agenta pro Linux najdete v tématu [připojit vaše počítače se systémem Linux tooOperations Management Suite (OMS)](log-analytics-agent-linux.md).</span><span class="sxs-lookup"><span data-stu-id="34f3d-259">For more information and steps on how tooinstall hello OMS Agent for Linux, see [Connect your Linux Computers tooOperations Management Suite (OMS)](log-analytics-agent-linux.md).</span></span>

### <a name="for-all-linux-container-hosts-including-coreos"></a><span data-ttu-id="34f3d-260">Pro všechny hostitele Linux kontejneru, včetně CoreOS</span><span class="sxs-lookup"><span data-stu-id="34f3d-260">For all Linux container hosts including CoreOS</span></span>

<span data-ttu-id="34f3d-261">Spusťte kontejner hello OMS, které chcete toomonitor.</span><span class="sxs-lookup"><span data-stu-id="34f3d-261">Start hello OMS container that you want toomonitor.</span></span> <span data-ttu-id="34f3d-262">Upravit a použít hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="34f3d-262">Modify and use hello following example:</span></span>

```
sudo docker run --privileged -d -v /var/run/docker.sock:/var/run/docker.sock -e WSID="your workspace id" -e KEY="your key" -h=`hostname` -p 127.0.0.1:25225:25225 --name="omsagent" --restart=always microsoft/oms
```

### <a name="for-all-azure-government-linux-container-hosts-including-coreos"></a><span data-ttu-id="34f3d-263">Pro všechny hostitele kontejner Azure Government Linux včetně CoreOS</span><span class="sxs-lookup"><span data-stu-id="34f3d-263">For all Azure Government Linux container hosts including CoreOS</span></span>

<span data-ttu-id="34f3d-264">Spusťte kontejner hello OMS, které chcete toomonitor.</span><span class="sxs-lookup"><span data-stu-id="34f3d-264">Start hello OMS container that you want toomonitor.</span></span> <span data-ttu-id="34f3d-265">Upravit a použít hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="34f3d-265">Modify and use hello following example:</span></span>

```
sudo docker run --privileged -d -v /var/run/docker.sock:/var/run/docker.sock -v /var/log:/var/log -e WSID="your workspace id" -e KEY="your key" -e DOMAIN="opinsights.azure.us" -p 127.0.0.1:25225:25225 -p 127.0.0.1:25224:25224/udp --name="omsagent" -h=`hostname` --restart=always microsoft/oms
```

### <a name="switching-from-using-an-installed-linux-agent-tooone-in-a-container"></a><span data-ttu-id="34f3d-266">Přepínání z pomocí nainstalovaného agenta tooone Linux v kontejneru</span><span class="sxs-lookup"><span data-stu-id="34f3d-266">Switching from using an installed Linux agent tooone in a container</span></span>
<span data-ttu-id="34f3d-267">Pokud dříve používá hello přímo nainstalovat agenta a chcete tooinstead použijte agenta spuštěného v kontejneru, musíte nejdřív odebrat hello OMS agenta pro Linux.</span><span class="sxs-lookup"><span data-stu-id="34f3d-267">If you previously used hello directly-installed agent and want tooinstead use an agent running in a container, you must first remove hello OMS Agent for Linux.</span></span> <span data-ttu-id="34f3d-268">V tématu [hello odinstalace agenta OMS pro Linux](log-analytics-agent-linux.md#uninstalling-the-oms-agent-for-linux) toounderstand, jak odinstalovat toosuccessfully hello agenta.</span><span class="sxs-lookup"><span data-stu-id="34f3d-268">See [Uninstalling hello OMS Agent for Linux](log-analytics-agent-linux.md#uninstalling-the-oms-agent-for-linux) toounderstand how toosuccessfully uninstall hello agent.</span></span>  

### <a name="configure-an-oms-agent-for-docker-swarm"></a><span data-ttu-id="34f3d-269">Konfigurace agenta OMS pro Docker Swarm</span><span class="sxs-lookup"><span data-stu-id="34f3d-269">Configure an OMS agent for Docker Swarm</span></span>

<span data-ttu-id="34f3d-270">Jako globální služby můžete spustit hello agenta OMS na Docker Swarm.</span><span class="sxs-lookup"><span data-stu-id="34f3d-270">You can run hello OMS Agent as a global service on Docker Swarm.</span></span> <span data-ttu-id="34f3d-271">Použijte následující informace toocreate služby agenta OMS hello.</span><span class="sxs-lookup"><span data-stu-id="34f3d-271">Use hello following information toocreate an OMS Agent service.</span></span> <span data-ttu-id="34f3d-272">Je třeba tooinsert ID pracovního prostoru OMS a primární klíč.</span><span class="sxs-lookup"><span data-stu-id="34f3d-272">You need tooinsert your OMS Workspace ID and Primary Key.</span></span>

- <span data-ttu-id="34f3d-273">Spusťte následující hello na hlavní uzel hello.</span><span class="sxs-lookup"><span data-stu-id="34f3d-273">Run hello following on hello master node.</span></span>

    ```
    sudo docker service create  --name omsagent --mode global  --mount type=bind,source=/var/run/docker.sock,destination=/var/run/docker.sock  -e WSID="<WORKSPACE ID>" -e KEY="<PRIMARY KEY>" -p 25225:25225 -p 25224:25224/udp  --restart-condition=on-failure microsoft/oms
    ```

### <a name="configure-an-oms-agent-for-red-hat-openshift"></a><span data-ttu-id="34f3d-274">Konfigurace agenta OMS pro OpenShift Red Hat</span><span class="sxs-lookup"><span data-stu-id="34f3d-274">Configure an OMS Agent for Red Hat OpenShift</span></span>
<span data-ttu-id="34f3d-275">Existují tři způsoby tooadd hello agenta OMS tooRed Hat OpenShift toostart shromažďování kontejner dat monitorování.</span><span class="sxs-lookup"><span data-stu-id="34f3d-275">There are three ways tooadd hello OMS Agent tooRed Hat OpenShift toostart collecting container monitoring data.</span></span>

* <span data-ttu-id="34f3d-276">[Instalace hello OMS agenta pro Linux](log-analytics-agent-linux.md) přímo na každém uzlu OpenShift</span><span class="sxs-lookup"><span data-stu-id="34f3d-276">[Install hello OMS Agent for Linux](log-analytics-agent-linux.md) directly on each OpenShift node</span></span>  
* <span data-ttu-id="34f3d-277">[Povolit rozšíření virtuálního počítače Log Analytics](log-analytics-azure-vm-extension.md) na každém uzlu OpenShift umístěných v Azure</span><span class="sxs-lookup"><span data-stu-id="34f3d-277">[Enable Log Analytics VM Extension](log-analytics-azure-vm-extension.md) on each OpenShift node residing in Azure</span></span>  
* <span data-ttu-id="34f3d-278">Nainstalovat agenta OMS hello jako OpenShift démon sadu</span><span class="sxs-lookup"><span data-stu-id="34f3d-278">Install hello OMS Agent as a OpenShift daemon-set</span></span>  

<span data-ttu-id="34f3d-279">V této části nabídneme hello kroky požadované tooinstall hello agenta OMS jako démon OpenShift-set.</span><span class="sxs-lookup"><span data-stu-id="34f3d-279">In this section we cover hello steps required tooinstall hello OMS Agent as an OpenShift daemon-set.</span></span>  

1. <span data-ttu-id="34f3d-280">Přihlášení toohello OpenShift hlavní uzel a zkopírujte hello yaml soubor [ocp-omsagent.yaml](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-omsagent.yaml) z Githubu tooyour hlavní uzel a měnit hodnotu hello s vaše ID pracovního prostoru OMS a primární klíč.</span><span class="sxs-lookup"><span data-stu-id="34f3d-280">Sign on toohello OpenShift master node and copy hello yaml file [ocp-omsagent.yaml](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-omsagent.yaml) from GitHub tooyour master node and modify hello value with your OMS Workspace ID and with your Primary Key.</span></span>
2. <span data-ttu-id="34f3d-281">Spusťte následující příkazy toocreate hello projektu pro OMS a nastavte hello uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="34f3d-281">Run hello following commands toocreate a project for OMS and set hello user account.</span></span>

    ```
    oadm new-project omslogging --node-selector='zone=default'
    oc project omslogging  
    oc create serviceaccount omsagent  
    oadm policy add-cluster-role-to-user cluster-reader   system:serviceaccount:omslogging:omsagent  
    oadm policy add-scc-to-user privileged system:serviceaccount:omslogging:omsagent  
    ```

4. <span data-ttu-id="34f3d-282">toodeploy hello démon set, spusťte následující hello:</span><span class="sxs-lookup"><span data-stu-id="34f3d-282">toodeploy hello daemon-set, run hello following:</span></span>

    `oc create -f ocp-omsagent.yaml`

5. <span data-ttu-id="34f3d-283">tooverify, který je nakonfigurován a že fungují správně, zadejte následující hello:</span><span class="sxs-lookup"><span data-stu-id="34f3d-283">tooverify it is configured and working correctly, type hello following:</span></span>

    `oc describe daemonset omsagent`  

    <span data-ttu-id="34f3d-284">a měl by vypadat jako výstup hello:</span><span class="sxs-lookup"><span data-stu-id="34f3d-284">and hello output should resemble:</span></span>

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

<span data-ttu-id="34f3d-285">Pokud chcete toouse tajné klíče toosecure ID pracovního prostoru OMS a primární klíč při použití souboru démon set yaml agenta OMS hello, proveďte následující kroky hello.</span><span class="sxs-lookup"><span data-stu-id="34f3d-285">If you want toouse secrets toosecure your OMS Workspace ID and Primary Key when using hello OMS Agent daemon-set yaml file, perform hello following steps.</span></span>

1. <span data-ttu-id="34f3d-286">Přihlášení toohello OpenShift hlavní uzel a zkopírujte hello yaml soubor [ocp-ds-omsagent.yaml](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-ds-omsagent.yaml) a tajný klíč generování skriptu [ocp-secretgen.sh](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-secretgen.sh) z Githubu.</span><span class="sxs-lookup"><span data-stu-id="34f3d-286">Sign on toohello OpenShift master node and copy hello yaml file [ocp-ds-omsagent.yaml](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-ds-omsagent.yaml) and secret generating script [ocp-secretgen.sh](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-secretgen.sh) from GitHub.</span></span>  <span data-ttu-id="34f3d-287">Tento skript vygeneruje soubor yaml hello tajné klíče pro ID pracovního prostoru OMS a primární klíč toosecure vaše secrete informace.</span><span class="sxs-lookup"><span data-stu-id="34f3d-287">This script will generate hello secrets yaml file for OMS Workspace ID and Primary Key toosecure your secrete information.</span></span>  
2. <span data-ttu-id="34f3d-288">Spusťte následující příkazy toocreate hello projektu pro OMS a nastavte hello uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="34f3d-288">Run hello following commands toocreate a project for OMS and set hello user account.</span></span> <span data-ttu-id="34f3d-289">tajný klíč Hello generování skriptu požádá o vaše ID pracovního prostoru OMS <WSID> a primární klíč <KEY> a po dokončení zpracování se vytvoří soubor ocp secret.yaml hello.</span><span class="sxs-lookup"><span data-stu-id="34f3d-289">hello secret generating script asks for your OMS Workspace ID <WSID> and Primary Key <KEY> and upon completion, it creates hello ocp-secret.yaml file.</span></span>  

    ```
    oadm new-project omslogging --node-selector='zone=default'  
    oc project omslogging  
    oc create serviceaccount omsagent  
    oadm policy add-cluster-role-to-user cluster-reader   system:serviceaccount:omslogging:omsagent  
    oadm policy add-scc-to-user privileged system:serviceaccount:omslogging:omsagent  
    ```

4. <span data-ttu-id="34f3d-290">Nasaďte soubor tajný hello spuštěním hello následující:</span><span class="sxs-lookup"><span data-stu-id="34f3d-290">Deploy hello secret file by running hello following:</span></span>

    `oc create -f ocp-secret.yaml`

5. <span data-ttu-id="34f3d-291">Ověření nasazení tak, že spustíte následující hello:</span><span class="sxs-lookup"><span data-stu-id="34f3d-291">Verify deployment by running hello following:</span></span>

    `oc describe secret omsagent-secret`  

    <span data-ttu-id="34f3d-292">a měl by vypadat jako výstup hello:</span><span class="sxs-lookup"><span data-stu-id="34f3d-292">and hello  output should resemble:</span></span>  

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

6. <span data-ttu-id="34f3d-293">Nasaďte soubor démon set yaml agenta OMS hello spuštěním hello následující:</span><span class="sxs-lookup"><span data-stu-id="34f3d-293">Deploy hello OMS Agent daemon-set yaml file by running hello following:</span></span>

    `oc create -f ocp-ds-omsagent.yaml`  

7. <span data-ttu-id="34f3d-294">Ověření nasazení tak, že spustíte následující hello:</span><span class="sxs-lookup"><span data-stu-id="34f3d-294">Verify deployment by running hello following:</span></span>

    `oc describe ds oms`

    <span data-ttu-id="34f3d-295">a měl by vypadat jako výstup hello:</span><span class="sxs-lookup"><span data-stu-id="34f3d-295">and hello output should resemble:</span></span>

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

### <a name="secure-your-secret-information-for-docker-swarm-and-kubernetes"></a><span data-ttu-id="34f3d-296">Zabezpečit vaše tajné informace pro Docker Swarm a Kubernetes</span><span class="sxs-lookup"><span data-stu-id="34f3d-296">Secure your secret information for Docker Swarm and Kubernetes</span></span>

<span data-ttu-id="34f3d-297">Pro Docker Swarm a Kubernetes kontejneru služby můžete zabezpečit tajný ID pracovního prostoru OMS a primární klíče.</span><span class="sxs-lookup"><span data-stu-id="34f3d-297">You can secure your secret OMS Workspace ID and Primary Keys for Docker Swarm and Kubernetes container services.</span></span>

#### <a name="secure-secrets-for-docker-swarm"></a><span data-ttu-id="34f3d-298">Zabezpečené tajné klíče pro Docker Swarm</span><span class="sxs-lookup"><span data-stu-id="34f3d-298">Secure secrets for Docker Swarm</span></span>

<span data-ttu-id="34f3d-299">Pro Docker Swarm, jakmile se vytvoří hello tajný klíč pro ID pracovního prostoru a primární klíč, můžete spustit hello vytvořte službu Docker hello OMSagent.</span><span class="sxs-lookup"><span data-stu-id="34f3d-299">For Docker Swarm, once hello secret for Workspace ID and Primary Key is created, you can run hello create hello Docker service for OMSagent.</span></span> <span data-ttu-id="34f3d-300">Použijte následující informace toocreate hello tajné informace.</span><span class="sxs-lookup"><span data-stu-id="34f3d-300">Use hello following information toocreate your secret information.</span></span>

1. <span data-ttu-id="34f3d-301">Spusťte následující hello na hlavní uzel hello.</span><span class="sxs-lookup"><span data-stu-id="34f3d-301">Run hello following on hello master node.</span></span>

    ```
    echo "WSID" | docker secret create WSID -
    echo "KEY" | docker secret create KEY -
    ```

2. <span data-ttu-id="34f3d-302">Ověřte, že tajné klíče byly vytvořeny správně.</span><span class="sxs-lookup"><span data-stu-id="34f3d-302">Verify that secrets were created properly.</span></span>

    ```
    keiko@swarmm-master-13957614-0:/run# sudo docker secret ls
    ```

    ```
    ID                          NAME                CREATED             UPDATED
    j2fj153zxy91j8zbcitnjxjiv   WSID                43 minutes ago      43 minutes ago
    l9rh3n987g9c45zffuxdxetd9   KEY                 38 minutes ago      38 minutes ago
    ```

3. <span data-ttu-id="34f3d-303">Spuštění hello následující příkaz toomount hello tajné klíče toohello kontejnerizované OMS Agent.</span><span class="sxs-lookup"><span data-stu-id="34f3d-303">Run hello following command toomount hello secrets toohello containerized OMS Agent.</span></span>

    ```
    sudo docker service create  --name omsagent --mode global  --mount type=bind,source=/var/run/docker.sock,destination=/var/run/docker.sock --secret source=WSID,target=WSID --secret source=KEY,target=KEY  -p 25225:25225 -p 25224:25224/udp --restart-condition=on-failure microsoft/oms
    ```

#### <a name="secure-secrets-for-kubernetes-with-yaml-files"></a><span data-ttu-id="34f3d-304">Zabezpečené tajné klíče pro Kubernetes s yaml soubory</span><span class="sxs-lookup"><span data-stu-id="34f3d-304">Secure secrets for Kubernetes with yaml files</span></span>

<span data-ttu-id="34f3d-305">Pro Kubernetes použijete soubor skriptu toogenerate hello tajné klíče yaml ID pracovního prostoru a primární klíč.</span><span class="sxs-lookup"><span data-stu-id="34f3d-305">For Kubernetes, you use a script toogenerate hello secrets yaml file for your Workspace ID and Primary Key.</span></span> <span data-ttu-id="34f3d-306">V hello [OMS Docker Kubernetes Githubu](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes) stránky, jsou soubory, které můžete použít s nebo bez tajné informace.</span><span class="sxs-lookup"><span data-stu-id="34f3d-306">At hello [OMS Docker Kubernetes GitHub](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes) page, there are files that you can use with or without your secret information.</span></span>

- <span data-ttu-id="34f3d-307">Hello výchozí DaemonSet agenta OMS nemá tajné informace (omsagent.yaml)</span><span class="sxs-lookup"><span data-stu-id="34f3d-307">hello Default OMS Agent DaemonSet does not have secret information (omsagent.yaml)</span></span>
- <span data-ttu-id="34f3d-308">soubor yaml DaemonSet agenta OMS Hello používá tajné informace (omsagent – ds-secrets.yaml) s tajný generování skriptů toogenerate hello tajné klíče yaml (omsagentsecret.yaml) souboru.</span><span class="sxs-lookup"><span data-stu-id="34f3d-308">hello OMS Agent DaemonSet yaml file uses secret information (omsagent-ds-secrets.yaml) with secret generation scripts toogenerate hello secrets yaml (omsagentsecret.yaml) file.</span></span>

<span data-ttu-id="34f3d-309">Můžete zvolit toocreate omsagent DaemonSets s nebo bez tajných klíčů.</span><span class="sxs-lookup"><span data-stu-id="34f3d-309">You can choose toocreate omsagent DaemonSets with or without secrets.</span></span>

##### <a name="default-omsagent-daemonset-yaml-file-without-secrets"></a><span data-ttu-id="34f3d-310">Výchozí soubor yaml OMSagent DaemonSet bez tajné klíče</span><span class="sxs-lookup"><span data-stu-id="34f3d-310">Default OMSagent DaemonSet yaml file without secrets</span></span>

- <span data-ttu-id="34f3d-311">Hello výchozí DaemonSet agenta OMS yaml souboru, nahraďte hello `<WSID>` a `<KEY>` tooyour WSID a klíč.</span><span class="sxs-lookup"><span data-stu-id="34f3d-311">For hello default OMS Agent DaemonSet yaml file, replace hello `<WSID>` and `<KEY>` tooyour WSID and KEY.</span></span> <span data-ttu-id="34f3d-312">Zkopírujte hlavní uzel tooyour hello souboru a spuštění hello následující:</span><span class="sxs-lookup"><span data-stu-id="34f3d-312">Copy hello file tooyour master node and run hello following:</span></span>

    ```
    sudo kubectl create -f omsagent.yaml
    ```

##### <a name="default-omsagent-daemonset-yaml-file-with-secrets"></a><span data-ttu-id="34f3d-313">Výchozí OMSagent DaemonSet yaml soubor obsahující tajné údaje</span><span class="sxs-lookup"><span data-stu-id="34f3d-313">Default OMSagent DaemonSet yaml file with secrets</span></span>

1. <span data-ttu-id="34f3d-314">toouse DaemonSet agenta OMS tajné informace, pomocí tajných klíčů hello nejprve vytvořit.</span><span class="sxs-lookup"><span data-stu-id="34f3d-314">toouse OMS Agent DaemonSet using secret information, create hello secrets first.</span></span>
    1. <span data-ttu-id="34f3d-315">Zkopírujte skript hello a soubor tajný šablony a ujistěte se, že jsou na hello stejný adresář.</span><span class="sxs-lookup"><span data-stu-id="34f3d-315">Copy hello script and secret template file and make sure they are on hello same directory.</span></span>
        - <span data-ttu-id="34f3d-316">Generování skriptu - tajný klíč gen.sh tajný klíč</span><span class="sxs-lookup"><span data-stu-id="34f3d-316">Secret generating script - secret-gen.sh</span></span>
        - <span data-ttu-id="34f3d-317">Šablona tajné – template.yaml tajný klíč</span><span class="sxs-lookup"><span data-stu-id="34f3d-317">secret template - secret-template.yaml</span></span>
    2. <span data-ttu-id="34f3d-318">Spusťte skript hello, jako je hello následující ukázka.</span><span class="sxs-lookup"><span data-stu-id="34f3d-318">Run hello script, like hello following example.</span></span> <span data-ttu-id="34f3d-319">skript Hello požádá o hello ID pracovního prostoru OMS a primární klíč a po zadání je skript hello vytvoří soubor tajný yaml, můžete ji spustit.</span><span class="sxs-lookup"><span data-stu-id="34f3d-319">hello script asks for hello OMS Workspace ID and Primary Key and after you enter them, hello script creates a secret yaml file so you can run it.</span></span>   

        ```
        #> sudo bash ./secret-gen.sh
        ```

    3. <span data-ttu-id="34f3d-320">Vytvořte hello tajné klíče pod spuštěním hello následující:</span><span class="sxs-lookup"><span data-stu-id="34f3d-320">Create hello secrets pod by running hello following:</span></span>
        ```
        sudo kubectl create -f omsagentsecret.yaml
        ```

    4. <span data-ttu-id="34f3d-321">tooverify spustit hello následující:</span><span class="sxs-lookup"><span data-stu-id="34f3d-321">tooverify, run hello following:</span></span>

        ```
        keiko@ubuntu16-13db:~# sudo kubectl get secrets
        ```

        <span data-ttu-id="34f3d-322">Výstup by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="34f3d-322">Output should resemble:</span></span>

        ```
        NAME                  TYPE                                  DATA      AGE
        default-token-gvl91   kubernetes.io/service-account-token   3         50d
        omsagent-secret       Opaque                                2         1d
        ```

        ```
        keiko@ubuntu16-13db:~# sudo kubectl describe secrets omsagent-secret
        ```

        <span data-ttu-id="34f3d-323">Výstup by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="34f3d-323">Output should resemble:</span></span>

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

    5. <span data-ttu-id="34f3d-324">Vytvoření vaší omsagent démon set spuštěním``` sudo kubectl create -f omsagent-ds-secrets.yaml ```</span><span class="sxs-lookup"><span data-stu-id="34f3d-324">Create your omsagent daemon-set by running ``` sudo kubectl create -f omsagent-ds-secrets.yaml ```</span></span>

2. <span data-ttu-id="34f3d-325">Ověřte, že hello, které běží DaemonSet agenta OMS, podobně jako toohello následující:</span><span class="sxs-lookup"><span data-stu-id="34f3d-325">Verify that hello OMS Agent DaemonSet is running, similar toohello following:</span></span>

    ```
    keiko@ubuntu16-13db:~# sudo kubectl get ds omsagent
    ```

    ```
    NAME       DESIRED   CURRENT   NODE-SELECTOR   AGE
    omsagent   3         3         <none>          1h
    ```


<span data-ttu-id="34f3d-326">Pro Kubernetes použijte soubor skriptu toogenerate hello tajné klíče yaml pro ID pracovního prostoru a primární klíč.</span><span class="sxs-lookup"><span data-stu-id="34f3d-326">For Kubernetes, use a script toogenerate hello secrets yaml file for Workspace ID and Primary Key.</span></span> <span data-ttu-id="34f3d-327">Hello použijte následující informace z příkladu s hello [omsagent yaml soubor](https://github.com/Microsoft/OMS-docker/blob/master/Kubernetes/omsagent.yaml) toosecure tajné informace.</span><span class="sxs-lookup"><span data-stu-id="34f3d-327">Use hello following example information with hello [omsagent yaml file](https://github.com/Microsoft/OMS-docker/blob/master/Kubernetes/omsagent.yaml) toosecure your secret information.</span></span>

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

## <a name="windows-container-hosts"></a><span data-ttu-id="34f3d-328">Hostitelé kontejneru systému Windows</span><span class="sxs-lookup"><span data-stu-id="34f3d-328">Windows container hosts</span></span>

### <a name="preparation-before-installing-windows-agents"></a><span data-ttu-id="34f3d-329">Příprava před instalací agentů v systému Windows</span><span class="sxs-lookup"><span data-stu-id="34f3d-329">Preparation before installing Windows agents</span></span>

<span data-ttu-id="34f3d-330">Před instalací agentů do počítačů se systémem Windows, musíte tooconfigure hello Docker služby.</span><span class="sxs-lookup"><span data-stu-id="34f3d-330">Before you install agents on computers running Windows, you need tooconfigure hello Docker service.</span></span> <span data-ttu-id="34f3d-331">Konfigurace Hello umožňuje hello Windows agenta nebo hello analýzy protokolů virtuálního počítače rozšíření toouse hello soketu Docker TCP tak, aby hello agentů můžete přistupovat vzdáleně hello Docker démon a toocapture data monitorování.</span><span class="sxs-lookup"><span data-stu-id="34f3d-331">hello configuration allows hello Windows agent or hello Log Analytics virtual machine extension toouse hello Docker TCP socket so that hello agents can access hello Docker daemon remotely and toocapture data for monitoring.</span></span>

#### <a name="toostart-docker-and-verify-its-configuration"></a><span data-ttu-id="34f3d-332">toostart Docker a ověřte jeho konfiguraci</span><span class="sxs-lookup"><span data-stu-id="34f3d-332">toostart Docker and verify its configuration</span></span>

<span data-ttu-id="34f3d-333">Existují kroky tooset až TCP pojmenovaný kanál pro systém Windows Server:</span><span class="sxs-lookup"><span data-stu-id="34f3d-333">There are steps needed tooset up TCP named pipe for Windows Server:</span></span>

1. <span data-ttu-id="34f3d-334">V prostředí Windows PowerShell povolte kanálu TCP a pojmenovaný kanál.</span><span class="sxs-lookup"><span data-stu-id="34f3d-334">In Windows PowerShell, enable TCP pipe and named pipe.</span></span>

    ```
    Stop-Service docker
    dockerd --unregister-service
    dockerd --register-service -H npipe:// -H 0.0.0.0:2375  
    Start-Service docker
    ```

2. <span data-ttu-id="34f3d-335">Nakonfigurujte Docker hello konfiguračního souboru pro TCP kanálu a pojmenovaný kanál.</span><span class="sxs-lookup"><span data-stu-id="34f3d-335">Configure Docker with hello configuration file for TCP pipe and named pipe.</span></span> <span data-ttu-id="34f3d-336">Hello konfigurační soubor se nachází v C:\ProgramData\docker\config\daemon.json.</span><span class="sxs-lookup"><span data-stu-id="34f3d-336">hello configuration file is located at C:\ProgramData\docker\config\daemon.json.</span></span>

    <span data-ttu-id="34f3d-337">V souboru daemon.json hello budete potřebovat následující hello:</span><span class="sxs-lookup"><span data-stu-id="34f3d-337">In hello daemon.json file, you will need hello following:</span></span>

    ```
    {
    "hosts": ["tcp://0.0.0.0:2375", "npipe://"]
    }
    ```

<span data-ttu-id="34f3d-338">Další informace o konfiguraci démon Docker hello používá s kontejnery Windows najdete v tématu [modulu Docker v systému Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon).</span><span class="sxs-lookup"><span data-stu-id="34f3d-338">For more information about hello Docker daemon configuration used with Windows Containers, see [Docker Engine on Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon).</span></span>


### <a name="install-windows-agents"></a><span data-ttu-id="34f3d-339">Instalace agentů v systému Windows</span><span class="sxs-lookup"><span data-stu-id="34f3d-339">Install Windows agents</span></span>

<span data-ttu-id="34f3d-340">tooenable Windows a monitorování, technologie Hyper-V kontejneru nainstalovat na počítačích s Windows, které jsou hostiteli kontejneru hello Microsoft Monitoring Agent (MMA).</span><span class="sxs-lookup"><span data-stu-id="34f3d-340">tooenable Windows and Hyper-V container monitoring, install hello Microsoft Monitoring Agent (MMA) on Windows computers that are container hosts.</span></span> <span data-ttu-id="34f3d-341">Pro počítače se systémem Windows v místním prostředí, najdete v části [tooLog počítače připojit Windows Analytics](log-analytics-windows-agents.md).</span><span class="sxs-lookup"><span data-stu-id="34f3d-341">For computers running Windows in your on-premises environment, see [Connect Windows computers tooLog Analytics](log-analytics-windows-agents.md).</span></span> <span data-ttu-id="34f3d-342">Pro virtuální počítače běžící v Azure, připojte je tooLog analýzy pomocí hello [rozšíření virtuálního počítače](log-analytics-azure-vm-extension.md).</span><span class="sxs-lookup"><span data-stu-id="34f3d-342">For virtual machines running in Azure, connect them tooLog Analytics using hello [virtual machine extension](log-analytics-azure-vm-extension.md).</span></span>

<span data-ttu-id="34f3d-343">Můžete monitorovat kontejnery Windows spuštěné v Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="34f3d-343">You can monitor Windows containers running on Service Fabric.</span></span> <span data-ttu-id="34f3d-344">Ale pouze [virtuální počítače běžící v Azure](log-analytics-azure-vm-extension.md) a [počítačů se systémem Windows v místním prostředí](log-analytics-windows-agents.md) jsou aktuálně podporovány pro Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="34f3d-344">However, only [virtual machines running in Azure](log-analytics-azure-vm-extension.md) and [computers running Windows in your on-premises environment](log-analytics-windows-agents.md) are currently supported for Service Fabric.</span></span>

<span data-ttu-id="34f3d-345">Můžete ověřit, jestli je že tento hello řešením pro monitorování kontejneru správně nastavený pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="34f3d-345">You can verify that hello Container Monitoring solution is set correctly for Windows.</span></span> <span data-ttu-id="34f3d-346">toocheck zda hello management pack nebyla stažení správně, vyhledejte *ContainerManagement.xxx*.</span><span class="sxs-lookup"><span data-stu-id="34f3d-346">toocheck whether hello management pack was download properly, look for *ContainerManagement.xxx*.</span></span> <span data-ttu-id="34f3d-347">Hello soubory musí být ve složce C:\Program Files\Microsoft Monitoring Agent\Agent\Health State\Management SP hello.</span><span class="sxs-lookup"><span data-stu-id="34f3d-347">hello files should be in hello C:\Program Files\Microsoft Monitoring Agent\Agent\Health Service State\Management Packs folder.</span></span>


## <a name="solution-components"></a><span data-ttu-id="34f3d-348">Součásti řešení</span><span class="sxs-lookup"><span data-stu-id="34f3d-348">Solution components</span></span>

<span data-ttu-id="34f3d-349">Pokud používáte agenty se systémem Windows, potom hello následující sady management pack je nainstalována na každém počítači s agentem při přidání tohoto řešení.</span><span class="sxs-lookup"><span data-stu-id="34f3d-349">If you are using Windows agents, then hello following management pack is installed on each computer with an agent when you add this solution.</span></span> <span data-ttu-id="34f3d-350">Je požadován pro sadu management pack hello žádná konfigurace nebo údržby.</span><span class="sxs-lookup"><span data-stu-id="34f3d-350">No configuration or maintenance is required for hello management pack.</span></span>

- <span data-ttu-id="34f3d-351">*ContainerManagement.xxx* nainstalované v C:\Program Files\Microsoft Monitoring Agent\Agent\Health State\Management SP</span><span class="sxs-lookup"><span data-stu-id="34f3d-351">*ContainerManagement.xxx* installed in C:\Program Files\Microsoft Monitoring Agent\Agent\Health Service State\Management Packs</span></span>

## <a name="container-data-collection-details"></a><span data-ttu-id="34f3d-352">Kontejner dat kolekce podrobnosti</span><span class="sxs-lookup"><span data-stu-id="34f3d-352">Container data collection details</span></span>
<span data-ttu-id="34f3d-353">Hello řešením pro monitorování kontejneru shromažďuje různé metriky a protokolu údaje o výkonu z kontejneru hostitelů a kontejnery pomocí agentů, které povolíte.</span><span class="sxs-lookup"><span data-stu-id="34f3d-353">hello Container Monitoring solution collects various performance metrics and log data from container hosts and containers using agents that you enable.</span></span>

<span data-ttu-id="34f3d-354">Shromažďuje data každé tři minuty hello následující typy agenta.</span><span class="sxs-lookup"><span data-stu-id="34f3d-354">Data is collected every three minutes by hello following agent types.</span></span>

- [<span data-ttu-id="34f3d-355">OMS agenta pro Linux</span><span class="sxs-lookup"><span data-stu-id="34f3d-355">OMS Agent for Linux</span></span>](log-analytics-linux-agents.md)
- [<span data-ttu-id="34f3d-356">Agent služby Windows</span><span class="sxs-lookup"><span data-stu-id="34f3d-356">Windows agent</span></span>](log-analytics-windows-agents.md)
- [<span data-ttu-id="34f3d-357">Rozšíření virtuálního počítače analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="34f3d-357">Log Analytics VM extension</span></span>](log-analytics-azure-vm-extension.md)


### <a name="container-records"></a><span data-ttu-id="34f3d-358">Záznamů kontejneru</span><span class="sxs-lookup"><span data-stu-id="34f3d-358">Container records</span></span>

<span data-ttu-id="34f3d-359">Hello následující tabulka uvádí příklady záznamů shromážděných řešením pro monitorování kontejneru hello a hello datové typy, které se zobrazí ve výsledcích hledání protokolu.</span><span class="sxs-lookup"><span data-stu-id="34f3d-359">hello following table shows examples of records collected by hello Container Monitoring solution and hello data types that appear in log search results.</span></span>

| <span data-ttu-id="34f3d-360">Datový typ</span><span class="sxs-lookup"><span data-stu-id="34f3d-360">Data type</span></span> | <span data-ttu-id="34f3d-361">Datový typ v hledání protokolů</span><span class="sxs-lookup"><span data-stu-id="34f3d-361">Data type in Log Search</span></span> | <span data-ttu-id="34f3d-362">Pole</span><span class="sxs-lookup"><span data-stu-id="34f3d-362">Fields</span></span> |
| --- | --- | --- |
| <span data-ttu-id="34f3d-363">Výkon pro hostitele a kontejnery</span><span class="sxs-lookup"><span data-stu-id="34f3d-363">Performance for hosts and containers</span></span> | `Type=Perf` | <span data-ttu-id="34f3d-364">Počítač, ObjectName, název_čítače &#40; % času procesoru, Disk načte MB, zapíše MB, MB využití paměti, disku sítě přijatých bajtů, síti odesílat bajtů, procesor doba využití sítě &#41; přepočtené, TimeGenerated, Cesta_k_čítači, SourceSystem</span><span class="sxs-lookup"><span data-stu-id="34f3d-364">Computer, ObjectName, CounterName &#40;%Processor Time, Disk Reads MB, Disk Writes MB, Memory Usage MB, Network Receive Bytes, Network Send Bytes, Processor Usage sec, Network&#41;, CounterValue,TimeGenerated, CounterPath, SourceSystem</span></span> |
| <span data-ttu-id="34f3d-365">Kontejner inventáře</span><span class="sxs-lookup"><span data-stu-id="34f3d-365">Container inventory</span></span> | `Type=ContainerInventory` | <span data-ttu-id="34f3d-366">TimeGenerated, počítače a název kontejneru, ContainerHostname, Image, ImageTag, ContainerState, ExitCode, EnvironmentVar, příkazu, CreatedTime, StartedTime, FinishedTime, SourceSystem, identifikátor ContainerID, ID obrázku</span><span class="sxs-lookup"><span data-stu-id="34f3d-366">TimeGenerated, Computer, container name, ContainerHostname, Image, ImageTag, ContainerState, ExitCode, EnvironmentVar, Command, CreatedTime, StartedTime, FinishedTime, SourceSystem, ContainerID, ImageID</span></span> |
| <span data-ttu-id="34f3d-367">Kontejner image inventáře</span><span class="sxs-lookup"><span data-stu-id="34f3d-367">Container image inventory</span></span> | `Type=ContainerImageInventory` | <span data-ttu-id="34f3d-368">TimeGenerated, počítače, Image, ImageTag, ImageSize, VirtualSize, spuštění, pozastavena, zastavit, se nezdařilo, SourceSystem, ID obrázku, TotalContainer</span><span class="sxs-lookup"><span data-stu-id="34f3d-368">TimeGenerated, Computer, Image, ImageTag, ImageSize, VirtualSize, Running, Paused, Stopped, Failed, SourceSystem, ImageID, TotalContainer</span></span> |
| <span data-ttu-id="34f3d-369">Kontejner protokolu</span><span class="sxs-lookup"><span data-stu-id="34f3d-369">Container log</span></span> | `Type=ContainerLog` | <span data-ttu-id="34f3d-370">TimeGenerated, počítač, ID bitové kopie, název kontejneru, LogEntrySource, LogEntry, SourceSystem, identifikátor ContainerID</span><span class="sxs-lookup"><span data-stu-id="34f3d-370">TimeGenerated, Computer, image ID, container name, LogEntrySource, LogEntry, SourceSystem, ContainerID</span></span> |
| <span data-ttu-id="34f3d-371">Protokol služby kontejneru</span><span class="sxs-lookup"><span data-stu-id="34f3d-371">Container service log</span></span> | `Type=ContainerServiceLog`  | <span data-ttu-id="34f3d-372">TimeGenerated, počítače, TimeOfCommand, Image, příkazu, SourceSystem, identifikátor ContainerID</span><span class="sxs-lookup"><span data-stu-id="34f3d-372">TimeGenerated, Computer, TimeOfCommand, Image, Command, SourceSystem, ContainerID</span></span> |
| <span data-ttu-id="34f3d-373">Uzel inventáře kontejneru</span><span class="sxs-lookup"><span data-stu-id="34f3d-373">Container node inventory</span></span> | `Type=ContainerNodeInventory_CL`| <span data-ttu-id="34f3d-374">TimeGenerated, počítače, ClassName_s, DockerVersion_s, OperatingSystem_s, Volume_s, Network_s, NodeRole_s, OrchestratorType_s, InstanceID_g, SourceSystem</span><span class="sxs-lookup"><span data-stu-id="34f3d-374">TimeGenerated, Computer, ClassName_s, DockerVersion_s, OperatingSystem_s, Volume_s, Network_s, NodeRole_s, OrchestratorType_s, InstanceID_g, SourceSystem</span></span>|
| <span data-ttu-id="34f3d-375">Kubernetes inventáře</span><span class="sxs-lookup"><span data-stu-id="34f3d-375">Kubernetes inventory</span></span> | `Type=KubePodInventory_CL` | <span data-ttu-id="34f3d-376">TimeGenerated, počítače, PodLabel_deployment_s, PodLabel_deploymentconfig_s, PodLabel_docker_registry_s, Name_s, Namespace_s, PodStatus_s, PodIp_s, PodUid_g, PodCreationTimeStamp_t, SourceSystem</span><span class="sxs-lookup"><span data-stu-id="34f3d-376">TimeGenerated, Computer, PodLabel_deployment_s, PodLabel_deploymentconfig_s, PodLabel_docker_registry_s, Name_s, Namespace_s, PodStatus_s, PodIp_s, PodUid_g, PodCreationTimeStamp_t, SourceSystem</span></span> |
| <span data-ttu-id="34f3d-377">Proces kontejneru</span><span class="sxs-lookup"><span data-stu-id="34f3d-377">Container process</span></span> | `Type=ContainerProcess_CL` | <span data-ttu-id="34f3d-378">TimeGenerated, počítače, Pod_s, Namespace_s, ClassName_s, InstanceID_s, Uid_s, PID_s, PPID_s, C_s, STIME_s, Tty_s, TIME_s, Cmd_s, Id_s, Name_s, SourceSystem</span><span class="sxs-lookup"><span data-stu-id="34f3d-378">TimeGenerated, Computer, Pod_s, Namespace_s, ClassName_s, InstanceID_s, Uid_s, PID_s, PPID_s, C_s, STIME_s, Tty_s, TIME_s, Cmd_s, Id_s, Name_s, SourceSystem</span></span> |
| <span data-ttu-id="34f3d-379">Kubernetes události</span><span class="sxs-lookup"><span data-stu-id="34f3d-379">Kubernetes events</span></span> | `Type=KubeEvents_CL` | <span data-ttu-id="34f3d-380">TimeGenerated, počítače, Name_s, ObjectKind_s, Namespace_s, Reason_s, Type_s, SourceComponent_s, SourceSystem, zprávy</span><span class="sxs-lookup"><span data-stu-id="34f3d-380">TimeGenerated, Computer, Name_s, ObjectKind_s, Namespace_s, Reason_s, Type_s, SourceComponent_s, SourceSystem, Message</span></span> |

<span data-ttu-id="34f3d-381">Popisky připojeno příliš*PodLabel* datové typy jsou vlastní štítky.</span><span class="sxs-lookup"><span data-stu-id="34f3d-381">Labels appended too*PodLabel* data types are your own custom labels.</span></span> <span data-ttu-id="34f3d-382">Hello připojením PodLabel popisky uvedené v tabulce hello jsou příklady.</span><span class="sxs-lookup"><span data-stu-id="34f3d-382">hello appended PodLabel labels shown in hello table are examples.</span></span> <span data-ttu-id="34f3d-383">Ano `PodLabel_deployment_s`, `PodLabel_deploymentconfig_s`, `PodLabel_docker_registry_s` se liší v sadě dat vaše prostředí a obecně vypadat jako `PodLabel_yourlabel_s`.</span><span class="sxs-lookup"><span data-stu-id="34f3d-383">So, `PodLabel_deployment_s`, `PodLabel_deploymentconfig_s`, `PodLabel_docker_registry_s` will differ in your environment's data set and generically resemble `PodLabel_yourlabel_s`.</span></span>


## <a name="monitor-containers"></a><span data-ttu-id="34f3d-384">Monitorování kontejnery</span><span class="sxs-lookup"><span data-stu-id="34f3d-384">Monitor containers</span></span>
<span data-ttu-id="34f3d-385">Až budete mít povoleno na portálu OMS hello řešení hello, hello **kontejnery** dlaždice se zobrazí souhrnné informace o kontejneru hostitelů a spuštěné v hostitelích kontejnery hello.</span><span class="sxs-lookup"><span data-stu-id="34f3d-385">After you have hello solution enabled in hello OMS portal, hello **Containers** tile shows summary information about your container hosts and hello containers running in hosts.</span></span>

![Dlaždice kontejnery](./media/log-analytics-containers/containers-title.png)

<span data-ttu-id="34f3d-387">dlaždice Hello ukazuje přehled o tom, kolik kontejnery, které máte v prostředí hello a jestli se nezdařila, spuštěná nebo zastavená.</span><span class="sxs-lookup"><span data-stu-id="34f3d-387">hello tile shows an overview of how many containers you have in hello environment and whether they're failed, running, or stopped.</span></span>

### <a name="using-hello-containers-dashboard"></a><span data-ttu-id="34f3d-388">Pomocí řídicího panelu hello kontejnery</span><span class="sxs-lookup"><span data-stu-id="34f3d-388">Using hello Containers dashboard</span></span>
<span data-ttu-id="34f3d-389">Klikněte na tlačítko hello **kontejnery** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="34f3d-389">Click hello **Containers** tile.</span></span> <span data-ttu-id="34f3d-390">Zde se zobrazí zobrazení uspořádané podle:</span><span class="sxs-lookup"><span data-stu-id="34f3d-390">From there you'll see views organized by:</span></span>

- <span data-ttu-id="34f3d-391">**Události kontejneru** -zobrazuje stav kontejneru a počítače s kontejnery se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="34f3d-391">**Container Events** - Shows container status and computers with failed containers.</span></span>
- <span data-ttu-id="34f3d-392">**Kontejner protokoly** – zobrazuje graf soubory protokolu kontejneru vygeneroval přes čas a seznam počítačů s hello nejvyšší počet souborů protokolu.</span><span class="sxs-lookup"><span data-stu-id="34f3d-392">**Container Logs** - Shows a chart of container log files generated over time and a list of computers with hello highest number of log files.</span></span>
- <span data-ttu-id="34f3d-393">**Události Kubernetes** – zobrazuje graf Kubernetes události generované přes čas a seznamu hello důvodů, proč pracovními stanicemi soustředěnými kolem vygeneruje hello události.</span><span class="sxs-lookup"><span data-stu-id="34f3d-393">**Kubernetes Events** - Shows a chart of Kubernetes events generated over time and a list of hello reasons why pods generated hello events.</span></span> <span data-ttu-id="34f3d-394">*Tato datová sada se používá jenom v prostředích Linux.*</span><span class="sxs-lookup"><span data-stu-id="34f3d-394">*This data set is used only in Linux environments.*</span></span>
- <span data-ttu-id="34f3d-395">**Kubernetes Namespace inventáře** – zobrazuje počet hello obory názvů a pracovními stanicemi soustředěnými kolem a zobrazí jejich hierarchie.</span><span class="sxs-lookup"><span data-stu-id="34f3d-395">**Kubernetes Namespace Inventory** - Shows hello number of namespaces and pods and shows their hierarchy.</span></span> <span data-ttu-id="34f3d-396">*Tato datová sada se používá jenom v prostředích Linux.*</span><span class="sxs-lookup"><span data-stu-id="34f3d-396">*This data set is used only in Linux environments.*</span></span>
- <span data-ttu-id="34f3d-397">**Uzel inventáře kontejneru** – zobrazuje počet hello orchestration typy používané v kontejneru uzly nebo hostitele.</span><span class="sxs-lookup"><span data-stu-id="34f3d-397">**Container Node Inventory** - Shows hello number of orchestration types used on container nodes/hosts.</span></span> <span data-ttu-id="34f3d-398">počítač Hello uzly nebo hostuje jsou také uvedeny podle hello počet kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="34f3d-398">hello computer nodes/hosts are also listed by hello number of containers.</span></span> <span data-ttu-id="34f3d-399">*Tato datová sada se používá jenom v prostředích Linux.*</span><span class="sxs-lookup"><span data-stu-id="34f3d-399">*This data set is used only in Linux environments.*</span></span>
- <span data-ttu-id="34f3d-400">**Kontejner Imagí inventáře** -zobrazuje celkový počet hello kontejneru obrázků použitých a počet typy obrázků.</span><span class="sxs-lookup"><span data-stu-id="34f3d-400">**Container Images Inventory** - Shows hello total number of container images used and number of image types.</span></span> <span data-ttu-id="34f3d-401">Hello počet bitových kopií, jsou také uvedené podle značky obrázku hello.</span><span class="sxs-lookup"><span data-stu-id="34f3d-401">hello number of images are also listed by hello image tag.</span></span>
- <span data-ttu-id="34f3d-402">**Kontejnery stav** -zobrazuje celkový počet kontejneru hello uzly nebo hostitelské počítače, které mají spuštěných kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="34f3d-402">**Containers Status** - Shows hello total number of container nodes/host computers that have running containers.</span></span> <span data-ttu-id="34f3d-403">Počítače jsou také uvedeny podle hello počet spuštěných hostitele.</span><span class="sxs-lookup"><span data-stu-id="34f3d-403">Computers are also listed by hello number of running hosts.</span></span>
- <span data-ttu-id="34f3d-404">**Kontejner proces** -zobrazuje spojnicový graf kontejneru procesů spuštěných v čase.</span><span class="sxs-lookup"><span data-stu-id="34f3d-404">**Container Process** - Shows a line chart of container processes running over time.</span></span> <span data-ttu-id="34f3d-405">Kontejnery také uvádí spuštěním příkazu/procesu v rámci kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="34f3d-405">Containers are also listed by running command/process within containers.</span></span> <span data-ttu-id="34f3d-406">*Tato datová sada se používá jenom v prostředích Linux.*</span><span class="sxs-lookup"><span data-stu-id="34f3d-406">*This data set is used only in Linux environments.*</span></span>
- <span data-ttu-id="34f3d-407">**Výkon procesoru kontejneru** – zobrazuje graf řádku hello průměrné využití procesoru v čase pro počítač uzly nebo hostitele.</span><span class="sxs-lookup"><span data-stu-id="34f3d-407">**Container CPU Performance** - Shows a line chart of hello average CPU utilization over time for computer nodes/hosts.</span></span> <span data-ttu-id="34f3d-408">Také seznamy hello počítače uzly nebo hostitelů na základě průměrné využití procesoru.</span><span class="sxs-lookup"><span data-stu-id="34f3d-408">Also lists hello computer nodes/hosts based on average CPU utilization.</span></span>
- <span data-ttu-id="34f3d-409">**Kontejner výkonu paměti** -znázorňuje spojnicový graf využití paměti v průběhu času.</span><span class="sxs-lookup"><span data-stu-id="34f3d-409">**Container Memory Performance** - Shows a line chart of memory usage over time.</span></span> <span data-ttu-id="34f3d-410">Také uvádí na název instance na základě využití paměti počítače.</span><span class="sxs-lookup"><span data-stu-id="34f3d-410">Also lists computer memory utilization based on instance name.</span></span>
- <span data-ttu-id="34f3d-411">**Výkon počítače** -znázorňuje spojnicových grafů hello procent výkonu procesoru v čase, procentuální využití paměti nad čas a MB volného místa na disku v průběhu času.</span><span class="sxs-lookup"><span data-stu-id="34f3d-411">**Computer Performance** - Shows line charts of hello percent of CPU performance over time, percent of memory usage over time, and megabytes of free disk space over time.</span></span> <span data-ttu-id="34f3d-412">Můžete ukazatel myši přesunete kterýkoli řádek v grafu tooview další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="34f3d-412">You can hover over any line in a chart tooview more details.</span></span>


<span data-ttu-id="34f3d-413">Každou oblast hello řídicí panel je vizuální reprezentace hledání, které běží na shromážděná data.</span><span class="sxs-lookup"><span data-stu-id="34f3d-413">Each area of hello dashboard is a visual representation of a search that is run on collected data.</span></span>

![Řídicí panel kontejnery](./media/log-analytics-containers/containers-dash01.png)

![Řídicí panel kontejnery](./media/log-analytics-containers/containers-dash02.png)

<span data-ttu-id="34f3d-416">V hello **stav kontejneru** oblast, klikněte na tlačítko hello hlavní oblasti, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="34f3d-416">In hello **Container Status** area, click hello top area, as shown below.</span></span>

![Stav kontejnery](./media/log-analytics-containers/containers-status.png)

<span data-ttu-id="34f3d-418">Protokol hledání se otevře, zobrazení informací o stavu hello kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="34f3d-418">Log Search opens, displaying information about hello state of your containers.</span></span>

![Hledání protokolů pro kontejnery](./media/log-analytics-containers/containers-log-search.png)

<span data-ttu-id="34f3d-420">Zde můžete upravit hello vyhledávání dotazu toomodify ho toofind hello konkrétní informace, co vás zajímá.</span><span class="sxs-lookup"><span data-stu-id="34f3d-420">From here, you can edit hello search query toomodify it toofind hello specific information you're interested in.</span></span> <span data-ttu-id="34f3d-421">Další informace o protokolu hledání najdete v tématu [přihlásit analýzy protokolů hledání](log-analytics-log-searches.md).</span><span class="sxs-lookup"><span data-stu-id="34f3d-421">For more information about Log Searches, see [Log searches in Log Analytics](log-analytics-log-searches.md).</span></span>

## <a name="troubleshoot-by-finding-a-failed-container"></a><span data-ttu-id="34f3d-422">Řešení potíží s tak, že najdete kontejner se nezdařilo</span><span class="sxs-lookup"><span data-stu-id="34f3d-422">Troubleshoot by finding a failed container</span></span>

<span data-ttu-id="34f3d-423">Analýzy protokolů označí kontejneru jako **se nezdařilo** Pokud byl ukončen s nenulový ukončovací kód.</span><span class="sxs-lookup"><span data-stu-id="34f3d-423">Log Analytics marks a container as **Failed** if it has exited with a non-zero exit code.</span></span> <span data-ttu-id="34f3d-424">Zobrazí se přehled hello chyb nebo selhání v prostředí hello hello **se nezdařilo kontejnery** oblasti.</span><span class="sxs-lookup"><span data-stu-id="34f3d-424">You can see an overview of hello errors and failures in hello environment in hello **Failed Containers** area.</span></span>

### <a name="toofind-failed-containers"></a><span data-ttu-id="34f3d-425">kontejnery toofind se nezdařilo</span><span class="sxs-lookup"><span data-stu-id="34f3d-425">toofind failed containers</span></span>
1. <span data-ttu-id="34f3d-426">Klikněte na tlačítko hello **stav kontejneru** oblasti.</span><span class="sxs-lookup"><span data-stu-id="34f3d-426">Click hello **Container Status** area.</span></span>  
   <span data-ttu-id="34f3d-427">![Stav kontejnery](./media/log-analytics-containers/containers-status.png)</span><span class="sxs-lookup"><span data-stu-id="34f3d-427">![containers status](./media/log-analytics-containers/containers-status.png)</span></span>
2. <span data-ttu-id="34f3d-428">Protokol hledání se zobrazí stav hello kontejnerů, podobně jako toohello následující.</span><span class="sxs-lookup"><span data-stu-id="34f3d-428">Log Search opens and displays hello state of your containers, similar toohello following.</span></span>  
   ![kontejnery stavu](./media/log-analytics-containers/containers-log-search.png)
3. <span data-ttu-id="34f3d-430">Klikněte na tlačítko hello agregovat hodnotu neúspěšné kontejnery tooview Další informace.</span><span class="sxs-lookup"><span data-stu-id="34f3d-430">Next, click hello aggregated value of failed containers tooview additional information.</span></span> <span data-ttu-id="34f3d-431">Rozbalte položku **zobrazit další** ID tooview hello obrázku.</span><span class="sxs-lookup"><span data-stu-id="34f3d-431">Expand **show more** tooview hello image ID.</span></span>  
   <span data-ttu-id="34f3d-432">![Neúspěšné kontejnery](./media/log-analytics-containers/containers-state-failed.png)</span><span class="sxs-lookup"><span data-stu-id="34f3d-432">![failed containers](./media/log-analytics-containers/containers-state-failed.png)</span></span>  
4. <span data-ttu-id="34f3d-433">Potom zadejte následující hello hello vyhledávací dotaz.</span><span class="sxs-lookup"><span data-stu-id="34f3d-433">Next, type hello following in hello search query.</span></span> <span data-ttu-id="34f3d-434">`Type=ContainerInventory <ImageID>`toosee údaje o hello image, jako je například velikost bitové kopie a počet zastaven a k selhání bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="34f3d-434">`Type=ContainerInventory <ImageID>` toosee details about hello image such as image size and number of stopped and failed images.</span></span>  
   <span data-ttu-id="34f3d-435">![Neúspěšné kontejnery](./media/log-analytics-containers/containers-failed04.png)</span><span class="sxs-lookup"><span data-stu-id="34f3d-435">![failed containers](./media/log-analytics-containers/containers-failed04.png)</span></span>

## <a name="search-logs-for-container-data"></a><span data-ttu-id="34f3d-436">Hledání protokoly pro kontejner dat</span><span class="sxs-lookup"><span data-stu-id="34f3d-436">Search logs for container data</span></span>
<span data-ttu-id="34f3d-437">Pokud se řešení potíží s konkrétní chyby, může pomoct toosee, kde je vznikl ve vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="34f3d-437">When you're troubleshooting a specific error, it can help toosee where it is occurring in your environment.</span></span> <span data-ttu-id="34f3d-438">následující typy protokolu Hello vám pomůže vytvořit dotazy tooreturn hello informace, které chcete.</span><span class="sxs-lookup"><span data-stu-id="34f3d-438">hello following log types will help you create queries tooreturn hello information you want.</span></span>


- <span data-ttu-id="34f3d-439">**ContainerImageInventory** – tento typ použijte, když se pokoušíte toofind informace uspořádané podle bitové kopie a tooview informace o obrázku jako ID bitové kopie nebo velikosti.</span><span class="sxs-lookup"><span data-stu-id="34f3d-439">**ContainerImageInventory** – Use this type when you're trying toofind information organized by image and tooview image information such as image IDs or sizes.</span></span>
- <span data-ttu-id="34f3d-440">**ContainerInventory** – tento typ použijte, pokud chcete informace o umístění kontejneru, jaké jsou jejich názvy a co bitové kopie, že používáte.</span><span class="sxs-lookup"><span data-stu-id="34f3d-440">**ContainerInventory** – Use this type when you want information about container location, what their names are, and what images they're running.</span></span>
- <span data-ttu-id="34f3d-441">**ContainerLog** – tento typ použijte, pokud chcete informace o protokolu konkrétní chybě toofind a položky.</span><span class="sxs-lookup"><span data-stu-id="34f3d-441">**ContainerLog** – Use this type when you want toofind specific error log information and entries.</span></span>
- <span data-ttu-id="34f3d-442">**ContainerNodeInventory_CL** tento typ můžete použít hello informace o uzlu hostitele nebo kde je umístěný kontejnery.</span><span class="sxs-lookup"><span data-stu-id="34f3d-442">**ContainerNodeInventory_CL**  Use this type when you want hello information about host/node where containers are residing.</span></span> <span data-ttu-id="34f3d-443">Poskytuje Docker verze, typ orchestration, úložiště a informace o síti.</span><span class="sxs-lookup"><span data-stu-id="34f3d-443">It provides you Docker version, orchestration type, storage, and network information.</span></span>
- <span data-ttu-id="34f3d-444">**ContainerProcess_CL** použít tento typ tooquickly najdete v části hello proces, který běží v rámci kontejneru hello.</span><span class="sxs-lookup"><span data-stu-id="34f3d-444">**ContainerProcess_CL** Use this type tooquickly see hello process running within hello container.</span></span>
- <span data-ttu-id="34f3d-445">**ContainerServiceLog** – tento typ použijte, pokud se pokoušíte toofind informace protokolu auditu pro hello Docker démona, jako je například spuštění, zastavení, odstraňovat nebo příkazy pro vyžádání obsahu.</span><span class="sxs-lookup"><span data-stu-id="34f3d-445">**ContainerServiceLog** – Use this type when you're trying toofind audit trail information for hello Docker daemon, such as start, stop, delete, or pull commands.</span></span>
- <span data-ttu-id="34f3d-446">**KubeEvents_CL** použít tento typ toosee hello Kubernetes události.</span><span class="sxs-lookup"><span data-stu-id="34f3d-446">**KubeEvents_CL**  Use this type toosee hello Kubernetes events.</span></span>
- <span data-ttu-id="34f3d-447">**KubePodInventory_CL** tento typ použijte, pokud chcete informace o hierarchii toounderstand hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="34f3d-447">**KubePodInventory_CL**  Use this type when you want toounderstand hello cluster hierarchy information.</span></span>


### <a name="toosearch-logs-for-container-data"></a><span data-ttu-id="34f3d-448">toosearch protokoly pro kontejner dat</span><span class="sxs-lookup"><span data-stu-id="34f3d-448">toosearch logs for container data</span></span>
* <span data-ttu-id="34f3d-449">Vyberte obrázek, který znáte selhával a vyhledání protokolů chyb hello pro ni.</span><span class="sxs-lookup"><span data-stu-id="34f3d-449">Choose an image that you know has failed recently and find hello error logs for it.</span></span> <span data-ttu-id="34f3d-450">Začněte tím, že název kontejneru, který běží této bitové kopie s hledání **ContainerInventory** vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="34f3d-450">Start by finding a container name that is running that image with a **ContainerInventory** search.</span></span> <span data-ttu-id="34f3d-451">Například vyhledejte`Type=ContainerInventory ubuntu Failed`</span><span class="sxs-lookup"><span data-stu-id="34f3d-451">For example, search for `Type=ContainerInventory ubuntu Failed`</span></span>  
    <span data-ttu-id="34f3d-452">![Hledat kontejnery Ubuntu](./media/log-analytics-containers/search-ubuntu.png)</span><span class="sxs-lookup"><span data-stu-id="34f3d-452">![Search for Ubuntu containers](./media/log-analytics-containers/search-ubuntu.png)</span></span>

  <span data-ttu-id="34f3d-453">Hello název kontejneru hello vedle příliš**název**a vyhledejte tyto protokoly.</span><span class="sxs-lookup"><span data-stu-id="34f3d-453">hello name of hello container next too**Name**, and search for those logs.</span></span> <span data-ttu-id="34f3d-454">V tomto příkladu je to `Type=ContainerLog cranky_stonebreaker`.</span><span class="sxs-lookup"><span data-stu-id="34f3d-454">In this example, it is `Type=ContainerLog cranky_stonebreaker`.</span></span>

<span data-ttu-id="34f3d-455">**Informace o zobrazení výkonu**</span><span class="sxs-lookup"><span data-stu-id="34f3d-455">**View performance information**</span></span>

<span data-ttu-id="34f3d-456">Pokud jste od tooconstruct dotazy, může pomoct toosee co je možné nejprve.</span><span class="sxs-lookup"><span data-stu-id="34f3d-456">When you're beginning tooconstruct queries, it can help toosee what's possible first.</span></span> <span data-ttu-id="34f3d-457">Například toosee, které vyhledávání všechny údaje o výkonu, zkuste široký dotazu zadáním hello následující dotaz.</span><span class="sxs-lookup"><span data-stu-id="34f3d-457">For example, toosee all performance data, try a broad query by typing hello following search query.</span></span>

```
Type=Perf
```

![kontejnery výkonu](./media/log-analytics-containers/containers-perf01.png)

<span data-ttu-id="34f3d-459">Data výkonu hello se zobrazuje specifický kontejner tooa zadáním názvu hello je toohello napravo od dotazu, můžete určit obor.</span><span class="sxs-lookup"><span data-stu-id="34f3d-459">You can scope hello performance data you're seeing tooa specific container by typing hello name of it toohello right of your query.</span></span>

```
Type=Perf <containerName>
```

<span data-ttu-id="34f3d-460">Který zobrazuje hello seznam metriky výkonu, které se shromažďují pro jednotlivé kontejneru.</span><span class="sxs-lookup"><span data-stu-id="34f3d-460">That shows hello list of performance metrics that are collected for an individual container.</span></span>

![kontejnery výkonu](./media/log-analytics-containers/containers-perf03.png)

## <a name="example-log-search-queries"></a><span data-ttu-id="34f3d-462">Příklad protokolu vyhledávací dotazy</span><span class="sxs-lookup"><span data-stu-id="34f3d-462">Example log search queries</span></span>
<span data-ttu-id="34f3d-463">Často užitečné toobuild dotazuje počínaje příklad nebo dva a úpravou těchto toofit prostředí.</span><span class="sxs-lookup"><span data-stu-id="34f3d-463">It's often useful toobuild queries starting with an example or two and then modifying them toofit your environment.</span></span> <span data-ttu-id="34f3d-464">Jako počáteční bod, můžete experimentovat s hello **ukázkové dotazy** oblasti toohelp sestavení složitější dotazy.</span><span class="sxs-lookup"><span data-stu-id="34f3d-464">As a starting point, you can experiment with hello **Sample Queries** area toohelp you build more advanced queries.</span></span>

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

![Kontejnery dotazy](./media/log-analytics-containers/containers-queries.png)


## <a name="saving-log-search-queries"></a><span data-ttu-id="34f3d-466">Uložení protokolu vyhledávacích dotazů</span><span class="sxs-lookup"><span data-stu-id="34f3d-466">Saving log search queries</span></span>
<span data-ttu-id="34f3d-467">Uložení dotazů je standardní funkce v analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="34f3d-467">Saving queries is a standard feature in Log Analytics.</span></span> <span data-ttu-id="34f3d-468">Je uložíte, budete mít ty, které jste najít užitečné užitečný pro budoucí použití.</span><span class="sxs-lookup"><span data-stu-id="34f3d-468">By saving them, you'll have those that you've found useful handy for future use.</span></span>

<span data-ttu-id="34f3d-469">Jakmile vytvoříte dotaz, který pro vás užitečné, uložte kliknutím na **Oblíbené** hello horní části stránky hledání protokolu hello.</span><span class="sxs-lookup"><span data-stu-id="34f3d-469">After you create a query that you find useful, save it by clicking **Favorites** at hello top of hello Log Search page.</span></span> <span data-ttu-id="34f3d-470">Potom můžete snadno k němu přístup později z hello **vlastní řídicí panel** stránky.</span><span class="sxs-lookup"><span data-stu-id="34f3d-470">Then you can easily access it later from hello **My Dashboard** page.</span></span>

## <a name="next-steps"></a><span data-ttu-id="34f3d-471">Další kroky</span><span class="sxs-lookup"><span data-stu-id="34f3d-471">Next steps</span></span>
* <span data-ttu-id="34f3d-472">[V protokolech Hledat](log-analytics-log-searches.md) tooview podrobné záznamy dat kontejneru.</span><span class="sxs-lookup"><span data-stu-id="34f3d-472">[Search logs](log-analytics-log-searches.md) tooview detailed container data records.</span></span>
