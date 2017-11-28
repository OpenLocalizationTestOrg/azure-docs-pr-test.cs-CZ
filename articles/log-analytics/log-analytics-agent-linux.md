---
title: "Připojení počítačů Linux k Operations Management Suite (OMS) | Microsoft Docs"
description: "Tento článek popisuje postup připojení počítače se systémem Linux hostované v Azure, ostatní cloudu nebo místně do OMS pomocí agenta OMS pro Linux."
services: log-analytics
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: 
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/21/2017
ms.author: magoedte
ms.openlocfilehash: 1c05f68235aafd0fa098a3b0edaba1258df09380
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="connect-your-linux-computers-to-operations-management-suite-oms"></a><span data-ttu-id="1a6c1-103">Připojení počítačů Linux k Operations Management Suite (OMS)</span><span class="sxs-lookup"><span data-stu-id="1a6c1-103">Connect your Linux Computers to Operations Management Suite (OMS)</span></span> 

<span data-ttu-id="1a6c1-104">S Microsoft Operations Management Suite (OMS) můžete shromažďovat a provádění akcí s data generovaná z počítače se systémem Linux a kontejner řešení jako Docker, umístěný ve vašem datovém centru místní jako fyzických serverech nebo virtuálních počítačů, virtuálních počítačů v Služba hostovaných v cloudu jako Amazon Web Services (AWS) nebo Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-104">With Microsoft Operations Management Suite (OMS), you can collect and act on data generated from Linux computers and container solutions like Docker, residing in your on-premises data center as physical servers or virtual machines, virtual machines in a cloud-hosted service like Amazon Web Services (AWS) or Microsoft Azure.</span></span> <span data-ttu-id="1a6c1-105">Také můžete řešení pro správu k dispozici v OMS například sledování změn, k identifikaci změny konfigurace a Správa aktualizací ke správě aktualizací softwaru k proaktivnímu řízení životního cyklu vaší virtuální počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-105">You can also use management solutions available in OMS such as Change Tracking, to identify configuration changes, and Update Management to manage software updates to proactively manage the lifecycle of your Linux VMs.</span></span> 

<span data-ttu-id="1a6c1-106">OMS agenta pro Linux komunikuje přes port 443 protokolu TCP odchozí službou OMS a pokud se počítač připojí k serveru brány firewall nebo proxy server komunikovat přes Internet, přečtěte si [konfigurace agenta pro použití s HTTP proxy server nebo OMS Brána](#configuring-the-agent-for-use-with-an-http-proxy-server-or-oms-gateway) pochopit změny konfigurace, které se musí provést.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-106">The OMS Agent for Linux communicates outbound with the OMS service over TCP port 443, and if the computer connects to a firewall or proxy server to communicate over the Internet, review [Configuring the agent for use with an HTTP proxy server or OMS Gateway](#configuring-the-agent-for-use-with-an-http-proxy-server-or-oms-gateway) to understand what configuration changes will need to be applied.</span></span>  <span data-ttu-id="1a6c1-107">Pokud sledujete počítači pomocí System Center 2016 - Operations Manager nebo Operations Manager 2012 R2, může být vícedomé službou OMS pro shromažďování dat a předání do služby a bude i nadále monitorovat pomocí nástroje Operations Manager.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-107">If you are monitoring the computer with System Center 2016 - Operations Manager or Operations Manager 2012 R2, it can be multi-homed with the OMS service to collect data and forward to the service and still be monitored by Operations Manager.</span></span>  <span data-ttu-id="1a6c1-108">Počítače se systémem Linux monitorovány podle skupiny pro správu nástroje Operations Manager, která je integrovaná s OMS neobdrží konfigurace zdroje dat a předávat shromážděné data prostřednictvím skupiny pro správu.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-108">Linux computers monitored by an Operations Manager management group that is integrated with OMS do not receive configuration for data sources and forward collected data through the management group.</span></span>  <span data-ttu-id="1a6c1-109">K více než jeden pracovní prostor sestavy nelze konfigurovat OMS agent.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-109">The OMS agent cannot be configured to report to more than one workspace.</span></span>  

<span data-ttu-id="1a6c1-110">Pokud vaše zásady zabezpečení IT neumožňují počítače v síti pro připojení k Internetu, agent může být nakonfigurován pro připojení k bráně OMS shromážděná data v závislosti na řešení, které jste povolili odesílat a přijímat informace o konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-110">If your IT security policies do not allow computers on your network to connect to the Internet, the agent can be configured to connect to the OMS Gateway to receive configuration information and send collected data depending on the solution you have enabled.</span></span> <span data-ttu-id="1a6c1-111">Další informace a kroky pro konfiguraci agenta OMS Linux komunikaci přes bránu OMS ke službě OMS najdete v tématu [počítače připojit k OMS pomocí brány OMS](log-analytics-oms-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="1a6c1-111">For more information and steps on how to configure your OMS Linux Agent to communicate through an OMS Gateway to the OMS service, see [Connect computers to OMS using the OMS Gateway](log-analytics-oms-gateway.md).</span></span>  

<span data-ttu-id="1a6c1-112">Následující diagram znázorňuje připojení mezi agentem spravované počítače se systémem Linux a OMS, včetně směr a portů.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-112">The following diagram depicts the connection between the agent-managed Linux computers and OMS, including the direction and ports.</span></span>

![agent přímé komunikaci s OMS diagram](./media/log-analytics-agent-linux/log-analytics-agent-linux-communication.png)

## <a name="system-requirements"></a><span data-ttu-id="1a6c1-114">Požadavky na systém</span><span class="sxs-lookup"><span data-stu-id="1a6c1-114">System requirements</span></span>
<span data-ttu-id="1a6c1-115">Než začnete, zkontrolujte následující podrobnosti k ověření, že splňujete požadavky.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-115">Before starting, review the following details to verify you meet the prerequisites.</span></span>

### <a name="supported-linux-operating-systems"></a><span data-ttu-id="1a6c1-116">Podporované operační systémy Linux</span><span class="sxs-lookup"><span data-stu-id="1a6c1-116">Supported Linux operating systems</span></span>
<span data-ttu-id="1a6c1-117">Následující Linuxových distribucích jsou oficiálně podporované.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-117">The following Linux distributions are officially supported.</span></span>  <span data-ttu-id="1a6c1-118">OMS agenta pro Linux mohou však spustit také na dalších distribuce, které nejsou uvedené.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-118">However, the OMS Agent for Linux might also run on other distributions not listed.</span></span>

* <span data-ttu-id="1a6c1-119">Linux Amazon 2012.09 k 2015.09 (x86/x64)</span><span class="sxs-lookup"><span data-stu-id="1a6c1-119">Amazon Linux 2012.09 to 2015.09 (x86/x64)</span></span>
* <span data-ttu-id="1a6c1-120">CentOS Linux 5, 6 a 7 (x86/x64)</span><span class="sxs-lookup"><span data-stu-id="1a6c1-120">CentOS Linux 5, 6, and 7 (x86/x64)</span></span>
* <span data-ttu-id="1a6c1-121">Oracle Linux 5, 6 a 7 (x86/x64)</span><span class="sxs-lookup"><span data-stu-id="1a6c1-121">Oracle Linux 5, 6, and 7 (x86/x64)</span></span>
* <span data-ttu-id="1a6c1-122">Red Hat Enterprise Linux Server 5, 6 a 7 (x86/x64)</span><span class="sxs-lookup"><span data-stu-id="1a6c1-122">Red Hat Enterprise Linux Server 5, 6 and 7 (x86/x64)</span></span>
* <span data-ttu-id="1a6c1-123">Debian GNU/Linux 6, 7 a 8 (x86/x64)</span><span class="sxs-lookup"><span data-stu-id="1a6c1-123">Debian GNU/Linux 6, 7, and 8 (x86/x64)</span></span>
* <span data-ttu-id="1a6c1-124">Ubuntu 12.04 LTS, 14.04 LTS, 15.04, 15.10, 16.04 LTS (x86/x64)</span><span class="sxs-lookup"><span data-stu-id="1a6c1-124">Ubuntu 12.04 LTS, 14.04 LTS, 15.04, 15.10, 16.04 LTS (x86/x64)</span></span>
* <span data-ttu-id="1a6c1-125">SUSE Linux Enterprise Server 11 a 12 (x86/x64)</span><span class="sxs-lookup"><span data-stu-id="1a6c1-125">SUSE Linux Enterprise Server 11 and 12 (x86/x64)</span></span>

### <a name="network"></a><span data-ttu-id="1a6c1-126">Síť</span><span class="sxs-lookup"><span data-stu-id="1a6c1-126">Network</span></span>
<span data-ttu-id="1a6c1-127">Informace o následující seznam konfigurace proxy a firewall informace požadované pro Linux agenta pro komunikaci s OMS.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-127">The information below list the proxy and firewall configuration information required for the Linux agent to communicate with OMS.</span></span> <span data-ttu-id="1a6c1-128">Přenosy jsou odchozí z vaší sítě do služby OMS.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-128">Traffic is outbound from your network to the OMS service.</span></span> 

|<span data-ttu-id="1a6c1-129">Prostředek agenta</span><span class="sxs-lookup"><span data-stu-id="1a6c1-129">Agent Resource</span></span>| <span data-ttu-id="1a6c1-130">Porty</span><span class="sxs-lookup"><span data-stu-id="1a6c1-130">Ports</span></span> |  
|------|---------|  
|<span data-ttu-id="1a6c1-131">*.ods.opinsights.azure.com</span><span class="sxs-lookup"><span data-stu-id="1a6c1-131">*.ods.opinsights.azure.com</span></span> | <span data-ttu-id="1a6c1-132">Port 443</span><span class="sxs-lookup"><span data-stu-id="1a6c1-132">Port 443</span></span>|   
|<span data-ttu-id="1a6c1-133">*.oms.opinsights.azure.com</span><span class="sxs-lookup"><span data-stu-id="1a6c1-133">*.oms.opinsights.azure.com</span></span> | <span data-ttu-id="1a6c1-134">Port 443</span><span class="sxs-lookup"><span data-stu-id="1a6c1-134">Port 443</span></span>|   
|<span data-ttu-id="1a6c1-135">*.BLOB.Core.Windows.NET/</span><span class="sxs-lookup"><span data-stu-id="1a6c1-135">*.blob.core.windows.net/</span></span> | <span data-ttu-id="1a6c1-136">Port 443</span><span class="sxs-lookup"><span data-stu-id="1a6c1-136">Port 443</span></span>|   
|<span data-ttu-id="1a6c1-137">*.azure-automation.net</span><span class="sxs-lookup"><span data-stu-id="1a6c1-137">*.azure-automation.net</span></span> | <span data-ttu-id="1a6c1-138">Port 443</span><span class="sxs-lookup"><span data-stu-id="1a6c1-138">Port 443</span></span>|  

### <a name="package-requirements"></a><span data-ttu-id="1a6c1-139">Požadavky na balíček</span><span class="sxs-lookup"><span data-stu-id="1a6c1-139">Package requirements</span></span>

 <span data-ttu-id="1a6c1-140">**Požadovaný balíček**</span><span class="sxs-lookup"><span data-stu-id="1a6c1-140">**Required package**</span></span>   | <span data-ttu-id="1a6c1-141">**Popis**</span><span class="sxs-lookup"><span data-stu-id="1a6c1-141">**Description**</span></span>   | <span data-ttu-id="1a6c1-142">**Minimální verze**</span><span class="sxs-lookup"><span data-stu-id="1a6c1-142">**Minimum version**</span></span>
--------------------- | --------------------- | -------------------
<span data-ttu-id="1a6c1-143">Glibc</span><span class="sxs-lookup"><span data-stu-id="1a6c1-143">Glibc</span></span> | <span data-ttu-id="1a6c1-144">Knihovny jazyka C GNU</span><span class="sxs-lookup"><span data-stu-id="1a6c1-144">GNU C Library</span></span>   | <span data-ttu-id="1a6c1-145">2.5-12</span><span class="sxs-lookup"><span data-stu-id="1a6c1-145">2.5-12</span></span> 
<span data-ttu-id="1a6c1-146">OpenSSL</span><span class="sxs-lookup"><span data-stu-id="1a6c1-146">Openssl</span></span> | <span data-ttu-id="1a6c1-147">Knihovny OpenSSL</span><span class="sxs-lookup"><span data-stu-id="1a6c1-147">OpenSSL Libraries</span></span> | <span data-ttu-id="1a6c1-148">0.9.8E nebo 1.0</span><span class="sxs-lookup"><span data-stu-id="1a6c1-148">0.9.8e or 1.0</span></span>
<span data-ttu-id="1a6c1-149">Curl</span><span class="sxs-lookup"><span data-stu-id="1a6c1-149">Curl</span></span> | <span data-ttu-id="1a6c1-150">cURL webového klienta</span><span class="sxs-lookup"><span data-stu-id="1a6c1-150">cURL web client</span></span> | <span data-ttu-id="1a6c1-151">7.15.5</span><span class="sxs-lookup"><span data-stu-id="1a6c1-151">7.15.5</span></span>
<span data-ttu-id="1a6c1-152">Python ctypes</span><span class="sxs-lookup"><span data-stu-id="1a6c1-152">Python-ctypes</span></span> | | 
<span data-ttu-id="1a6c1-153">PAM</span><span class="sxs-lookup"><span data-stu-id="1a6c1-153">PAM</span></span> | <span data-ttu-id="1a6c1-154">PAM moduly</span><span class="sxs-lookup"><span data-stu-id="1a6c1-154">Pluggable authentication Modules</span></span> | 

> [!NOTE]
>  <span data-ttu-id="1a6c1-155">Rsyslog nebo syslog ng jsou požadované ke shromáždění zprávy syslog.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-155">Either rsyslog or syslog-ng are required to collect syslog messages.</span></span> <span data-ttu-id="1a6c1-156">Démon procesu syslog výchozí na verze 5 Red Hat Enterprise Linux a CentOS, Oracle Linux verze (sysklog) není podporována pro shromažďování událostí syslog.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-156">The default syslog daemon on version 5 of Red Hat Enterprise Linux, CentOS, and Oracle Linux version (sysklog) is not supported for syslog event collection.</span></span> <span data-ttu-id="1a6c1-157">Ke shromažďování dat syslog z této verze těchto distribuce, by měl být démon rsyslog nainstalovaná a nakonfigurovaná nahradit sysklog,</span><span class="sxs-lookup"><span data-stu-id="1a6c1-157">To collect syslog data from this version of these distributions, the rsyslog daemon should be installed and configured to replace sysklog,</span></span> 

<span data-ttu-id="1a6c1-158">Agent obsahuje více balíčků.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-158">The agent includes multiple packages.</span></span> <span data-ttu-id="1a6c1-159">Verze souboru obsahuje následující balíčky k dispozici spuštěním sady prostředí s `--extract`:</span><span class="sxs-lookup"><span data-stu-id="1a6c1-159">The release file contains the following packages, available by running the shell bundle with `--extract`:</span></span>

<span data-ttu-id="1a6c1-160">**Balíček**</span><span class="sxs-lookup"><span data-stu-id="1a6c1-160">**Package**</span></span> | <span data-ttu-id="1a6c1-161">**Verze**</span><span class="sxs-lookup"><span data-stu-id="1a6c1-161">**Version**</span></span> | <span data-ttu-id="1a6c1-162">**Popis**</span><span class="sxs-lookup"><span data-stu-id="1a6c1-162">**Description**</span></span>
----------- | ----------- | --------------
<span data-ttu-id="1a6c1-163">omsagent</span><span class="sxs-lookup"><span data-stu-id="1a6c1-163">omsagent</span></span> | <span data-ttu-id="1a6c1-164">1.4.0</span><span class="sxs-lookup"><span data-stu-id="1a6c1-164">1.4.0</span></span> | <span data-ttu-id="1a6c1-165">Agent nástroje Operations Management Suite pro Linux</span><span class="sxs-lookup"><span data-stu-id="1a6c1-165">The Operations Management Suite Agent for Linux</span></span>
<span data-ttu-id="1a6c1-166">omsconfig</span><span class="sxs-lookup"><span data-stu-id="1a6c1-166">omsconfig</span></span> | <span data-ttu-id="1a6c1-167">1.1.1</span><span class="sxs-lookup"><span data-stu-id="1a6c1-167">1.1.1</span></span> | <span data-ttu-id="1a6c1-168">Konfigurace agenta pro agenta OMS</span><span class="sxs-lookup"><span data-stu-id="1a6c1-168">Configuration agent for the OMS Agent</span></span>
<span data-ttu-id="1a6c1-169">OMI</span><span class="sxs-lookup"><span data-stu-id="1a6c1-169">omi</span></span> | <span data-ttu-id="1a6c1-170">1.2.0</span><span class="sxs-lookup"><span data-stu-id="1a6c1-170">1.2.0</span></span> | <span data-ttu-id="1a6c1-171">Open Management Infrastructure (OMI) - lightweight CIM Server</span><span class="sxs-lookup"><span data-stu-id="1a6c1-171">Open Management Infrastructure (OMI) - a lightweight CIM Server</span></span>
<span data-ttu-id="1a6c1-172">scx.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-172">scx</span></span> | <span data-ttu-id="1a6c1-173">1.6.3</span><span class="sxs-lookup"><span data-stu-id="1a6c1-173">1.6.3</span></span> | <span data-ttu-id="1a6c1-174">Zprostředkovatelé CIM OMI pro metriku výkonu operačního systému</span><span class="sxs-lookup"><span data-stu-id="1a6c1-174">OMI CIM Providers for operating system performance metrics</span></span>
<span data-ttu-id="1a6c1-175">Apache cimprov</span><span class="sxs-lookup"><span data-stu-id="1a6c1-175">apache-cimprov</span></span> | <span data-ttu-id="1a6c1-176">1.0.1</span><span class="sxs-lookup"><span data-stu-id="1a6c1-176">1.0.1</span></span> | <span data-ttu-id="1a6c1-177">Zprostředkovatel pro OMI monitorování výkonu serveru Apache HTTP Server.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-177">Apache HTTP Server performance monitoring provider for OMI.</span></span> <span data-ttu-id="1a6c1-178">Nainstalovat, pokud je zjištěn serveru Apache HTTP Server.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-178">Installed if Apache HTTP Server is detected.</span></span>
<span data-ttu-id="1a6c1-179">MySQL cimprov</span><span class="sxs-lookup"><span data-stu-id="1a6c1-179">mysql-cimprov</span></span> | <span data-ttu-id="1a6c1-180">1.0.1</span><span class="sxs-lookup"><span data-stu-id="1a6c1-180">1.0.1</span></span> | <span data-ttu-id="1a6c1-181">Výkon serveru MySQL monitorování zprostředkovatele pro OMI.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-181">MySQL Server performance monitoring provider for OMI.</span></span> <span data-ttu-id="1a6c1-182">Nainstalovat, pokud je zjištěn MySQL nebo MariaDB server.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-182">Installed if MySQL/MariaDB server is detected.</span></span>
<span data-ttu-id="1a6c1-183">docker cimprov</span><span class="sxs-lookup"><span data-stu-id="1a6c1-183">docker-cimprov</span></span> | <span data-ttu-id="1a6c1-184">1.0.0</span><span class="sxs-lookup"><span data-stu-id="1a6c1-184">1.0.0</span></span> | <span data-ttu-id="1a6c1-185">Zprostředkovatel docker pro OMI.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-185">Docker provider for OMI.</span></span> <span data-ttu-id="1a6c1-186">Nainstalovat, pokud je zjištěn Docker.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-186">Installed if Docker is detected.</span></span>

### <a name="compatibility-with-system-center-operations-manager"></a><span data-ttu-id="1a6c1-187">Kompatibilita s nástrojem System Center Operations Manager</span><span class="sxs-lookup"><span data-stu-id="1a6c1-187">Compatibility with System Center Operations Manager</span></span>
<span data-ttu-id="1a6c1-188">OMS agenta pro Linux sdílí binárních souborů agenta s agenta System Center Operations Manager.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-188">The OMS Agent for Linux shares agent binaries with the System Center Operations Manager agent.</span></span> <span data-ttu-id="1a6c1-189">Pokud instalujete agenta OMS pro Linux v systému aktuálně spravován nástrojem Operations Manager, je OMI a SCX balíčky v počítači na novější verzi.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-189">If you install the OMS Agent for Linux on a system currently managed by Operations Manager, it the OMI and SCX packages on the computer to a newer version.</span></span> <span data-ttu-id="1a6c1-190">V této verzi OMS a System Center 2016 - Operations Manager, Operations Manager 2012 R2 agenti pro Linux jsou kompatibilní.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-190">In this release, the OMS and System Center 2016 - Operations Manager/Operations Manager 2012 R2 agents for Linux are compatible.</span></span> 

> [!NOTE]
> <span data-ttu-id="1a6c1-191">System Center 2012 SP1 a starší verze nejsou aktuálně kompatibilní nebo není podporován s agentem OMS pro Linux.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-191">System Center 2012 SP1 and earlier versions are currently not compatible or supported with the OMS Agent for Linux.</span></span><br>
> <span data-ttu-id="1a6c1-192">Pokud je Agent OMS pro Linux nainstalován do počítače, který není aktuálně sledovaných nástrojem Operations Manager a potom chcete monitorovat počítač s nástrojem Operations Manager, musíte upravit [OMI konfigurace](#enable-the-oms-agent-for-linux-to-report-to-system-center-operations-manager) před zjišťování počítač.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-192">If the OMS Agent for Linux is installed to a computer that is not currently monitored by Operations Manager, and you then wish to monitor the computer with Operations Manager, you must modify the [OMI configuration](#enable-the-oms-agent-for-linux-to-report-to-system-center-operations-manager) prior to discovering the computer.</span></span> <span data-ttu-id="1a6c1-193">**Tento krok je *není* nutný v případě, že je nainstalován agent nástroje Operations Manager před OMS agenta pro Linux.**</span><span class="sxs-lookup"><span data-stu-id="1a6c1-193">**This step is *not* needed if the Operations Manager agent is installed before the OMS Agent for Linux.**</span></span>

### <a name="system-configuration-changes"></a><span data-ttu-id="1a6c1-194">Změny konfigurace systému</span><span class="sxs-lookup"><span data-stu-id="1a6c1-194">System configuration changes</span></span>
<span data-ttu-id="1a6c1-195">Po instalaci agenta OMS pro balíčky Linux, se použijí následující další konfiguraci systémové změny.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-195">After installing the OMS Agent for Linux packages, the following additional system-wide configuration changes are applied.</span></span> <span data-ttu-id="1a6c1-196">Tyto artefakty se odeberou, když omsagent balíček odinstalován.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-196">These artifacts are removed when the omsagent package is uninstalled.</span></span>

* <span data-ttu-id="1a6c1-197">Bez oprávnění uživatele s názvem: `omsagent` je vytvořena.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-197">A non-privileged user named: `omsagent` is created.</span></span> <span data-ttu-id="1a6c1-198">Toto je účet, který spouští démon omsagent jako.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-198">This is the account the omsagent daemon runs as.</span></span>
* <span data-ttu-id="1a6c1-199">Soubor "zahrnout" sudoers je vytvořena při /etc/sudoers.d/omsagent.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-199">A sudoers “include” file is created at /etc/sudoers.d/omsagent.</span></span> <span data-ttu-id="1a6c1-200">To autorizuje omsagent restartovat démoni syslog a omsagent.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-200">This authorizes omsagent to restart the syslog and omsagent daemons.</span></span> <span data-ttu-id="1a6c1-201">Pokud direktivy "zahrnout" sudo nejsou podporovány v nainstalované verzi sudo, zapíšou se do /etc/sudoers tyto položky.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-201">If sudo “include” directives are not supported in the installed version of sudo, these entries are written to /etc/sudoers.</span></span>
* <span data-ttu-id="1a6c1-202">Konfigurace syslog je upravit tak, aby předávat podmnožinu události agenta.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-202">The syslog configuration is modified to forward a subset of events to the agent.</span></span> <span data-ttu-id="1a6c1-203">Další informace najdete v tématu **konfigurace shromažďování dat** části</span><span class="sxs-lookup"><span data-stu-id="1a6c1-203">For more information, see the **Configuring Data Collection** section below</span></span>

### <a name="upgrade-from-a-previous-release"></a><span data-ttu-id="1a6c1-204">Upgrade z předchozí verze</span><span class="sxs-lookup"><span data-stu-id="1a6c1-204">Upgrade from a previous release</span></span>
<span data-ttu-id="1a6c1-205">Upgrade z verze dříve, než je 1.0.0-47 v této verzi podporovány.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-205">Upgrade from versions earlier than 1.0.0-47 is supported in this release.</span></span> <span data-ttu-id="1a6c1-206">Instalace se `--upgrade` příkaz upgraduje všechny komponenty agenta na nejnovější verzi.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-206">Performing the installation with the `--upgrade` command upgrades all components of the agent to the latest version.</span></span>

## <a name="installing-the-agent"></a><span data-ttu-id="1a6c1-207">Instalace agenta</span><span class="sxs-lookup"><span data-stu-id="1a6c1-207">Installing the agent</span></span>

<span data-ttu-id="1a6c1-208">Tato část popisuje, jak nainstalovat agenta OMS pro Linux pomocí bunndle, který obsahuje Debian a RPM balíčky pro všechny komponenty agenta.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-208">This section describes how to install the OMS Agent for Linux using a bunndle, which contains Debian and RPM packages for each of the agent components.</span></span>  <span data-ttu-id="1a6c1-209">Můžete nainstalovat přímo nebo extrahují pro načtení daných jednotlivých balíčků.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-209">It can be installed directly or extracted to retrieve the individual packages.</span></span>  

<span data-ttu-id="1a6c1-210">Nejprve je třeba vaše OMS ID a klíč, který můžete najít a přepnutí na [klasický portál OMS](https://mms.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="1a6c1-210">First you need your OMS workspace ID and key, which you can find by switching to the [OMS classic portal](https://mms.microsoft.com).</span></span>  <span data-ttu-id="1a6c1-211">Na **přehled** stránky z hlavní nabídky vyberte možnost **nastavení**a potom přejděte na **připojené servery Sources\Linux**.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-211">On the **Overview** page, from the top menu select **Settings**, and then navigate to **Connected Sources\Linux Servers**.</span></span>  <span data-ttu-id="1a6c1-212">Zobrazí hodnotu napravo od **ID pracovního prostoru** a **primární klíč**.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-212">You see the value to the right of **Workspace ID** and **Primary Key**.</span></span>  <span data-ttu-id="1a6c1-213">Zkopírujte a vložte do vašeho oblíbeného editoru.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-213">Copy and paste both into your favorite editor.</span></span>    

1. <span data-ttu-id="1a6c1-214">Stáhněte si nejnovější [OMS agenta pro Linux (x64)](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/download/OMSAgent_GA_v1.4.0-45/omsagent-1.4.0-45.universal.x64.sh) nebo [OMS agenta pro Linux x86](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/download/OMSAgent_GA_v1.4.0-45/omsagent-1.4.0-45.universal.x86.sh) z Githubu.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-214">Download the latest [OMS Agent for Linux (x64)](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/download/OMSAgent_GA_v1.4.0-45/omsagent-1.4.0-45.universal.x64.sh) or [OMS Agent for Linux x86](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/download/OMSAgent_GA_v1.4.0-45/omsagent-1.4.0-45.universal.x86.sh) from GitHub.</span></span>  
2. <span data-ttu-id="1a6c1-215">Přeneste příslušné sady (x86 nebo x64) do počítače Linux pomocí spojovací bod služby/sftp.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-215">Transfer the appropriate bundle (x86 or x64) to your Linux computer using scp/sftp.</span></span>
3. <span data-ttu-id="1a6c1-216">Instalaci sady pomocí `--install` nebo `--upgrade` argument.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-216">Install the bundle by using the `--install` or `--upgrade` argument.</span></span> 

    > [!NOTE]
    > <span data-ttu-id="1a6c1-217">Pokud například když je již nainstalován agent nástroje System Center Operations Manager pro Linux jsou nainstalovány všechny existující balíčky, použijte `--upgrade` argument.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-217">If any existing packages are installed such as when the System Center Operations Manager agent for Linux is already installed, use the `--upgrade` argument.</span></span> <span data-ttu-id="1a6c1-218">Při instalaci připojovala k Operations Management Suite, zadejte `-w <WorkspaceID>` a `-s <Shared Key>` parametry.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-218">To connect to Operations Management Suite during installation, provide the `-w <WorkspaceID>` and `-s <Shared Key>` parameters.</span></span>


#### <a name="to-install-and-onboard-directly"></a><span data-ttu-id="1a6c1-219">Chcete-li nainstalovat a připojit přímo</span><span class="sxs-lookup"><span data-stu-id="1a6c1-219">To install and onboard directly</span></span>
```
sudo sh ./omsagent-<version>.universal.x64.sh --upgrade -w <workspace id> -s <shared key>
```

#### <a name="to-upgrade-the-agent-package"></a><span data-ttu-id="1a6c1-220">Upgradovat balíček agenta</span><span class="sxs-lookup"><span data-stu-id="1a6c1-220">To upgrade the agent package</span></span>
```
sudo sh ./omsagent-<version>.universal.x64.sh --upgrade
```

#### <a name="to-install-and-onboard-to-a-workspace-in-us-government-cloud"></a><span data-ttu-id="1a6c1-221">Chcete-li nainstalovat a zařadit do pracovního prostoru v Cloud vlády USA</span><span class="sxs-lookup"><span data-stu-id="1a6c1-221">To install and onboard to a workspace in US Government Cloud</span></span>
```
sudo sh ./omsagent-<version>.universal.x64.sh --upgrade -w <workspace id> -s <shared key> -d opinsights.azure.us
```

## <a name="configuring-the-agent-for-use-with-an-http-proxy-server-or-oms-gateway"></a><span data-ttu-id="1a6c1-222">Konfigurace agenta pro použití s OMS brány nebo HTTP proxy server</span><span class="sxs-lookup"><span data-stu-id="1a6c1-222">Configuring the agent for use with an HTTP proxy server or OMS Gateway</span></span>
<span data-ttu-id="1a6c1-223">OMS agenta pro Linux podporuje komunikaci buď prostřednictvím protokolu HTTP nebo HTTPS proxy serveru nebo brány OMS ke službě OMS.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-223">The OMS Agent for Linux supports communicating either through an HTTP or HTTPS proxy server or OMS Gateway to the OMS service.</span></span>  <span data-ttu-id="1a6c1-224">Anonymní i základní ověřování (uživatelské jméno a heslo) je podporováno.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-224">Both anonymous and basic authentication (username/password) is supported.</span></span>  

### <a name="proxy-configuration"></a><span data-ttu-id="1a6c1-225">Konfigurace proxy serveru</span><span class="sxs-lookup"><span data-stu-id="1a6c1-225">Proxy configuration</span></span>
<span data-ttu-id="1a6c1-226">Hodnota konfigurace proxy serveru má následující syntaxi:</span><span class="sxs-lookup"><span data-stu-id="1a6c1-226">The proxy configuration value has the following syntax:</span></span>

`[protocol://][user:password@]proxyhost[:port]`

<span data-ttu-id="1a6c1-227">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="1a6c1-227">Property</span></span>|<span data-ttu-id="1a6c1-228">Popis</span><span class="sxs-lookup"><span data-stu-id="1a6c1-228">Description</span></span>
-|-
<span data-ttu-id="1a6c1-229">Protocol (Protokol)</span><span class="sxs-lookup"><span data-stu-id="1a6c1-229">Protocol</span></span>|<span data-ttu-id="1a6c1-230">protokol HTTP nebo https</span><span class="sxs-lookup"><span data-stu-id="1a6c1-230">http or https</span></span>
<span data-ttu-id="1a6c1-231">Uživatel</span><span class="sxs-lookup"><span data-stu-id="1a6c1-231">user</span></span>|<span data-ttu-id="1a6c1-232">Volitelné uživatelské jméno pro ověření proxy serverem</span><span class="sxs-lookup"><span data-stu-id="1a6c1-232">Optional username for proxy authentication</span></span>
<span data-ttu-id="1a6c1-233">heslo</span><span class="sxs-lookup"><span data-stu-id="1a6c1-233">password</span></span>|<span data-ttu-id="1a6c1-234">Volitelné heslo pro ověření proxy serverem</span><span class="sxs-lookup"><span data-stu-id="1a6c1-234">Optional password for proxy authentication</span></span>
<span data-ttu-id="1a6c1-235">proxyhost</span><span class="sxs-lookup"><span data-stu-id="1a6c1-235">proxyhost</span></span>|<span data-ttu-id="1a6c1-236">Adresa nebo plně kvalifikovaný název domény serveru nebo OMS proxy serveru brány</span><span class="sxs-lookup"><span data-stu-id="1a6c1-236">Address or FQDN of the proxy server/OMS Gateway</span></span>
<span data-ttu-id="1a6c1-237">port</span><span class="sxs-lookup"><span data-stu-id="1a6c1-237">port</span></span>|<span data-ttu-id="1a6c1-238">Číslo portu volitelné pro server/OMS proxy serveru brány</span><span class="sxs-lookup"><span data-stu-id="1a6c1-238">Optional port number for the proxy server/OMS Gateway</span></span>

<span data-ttu-id="1a6c1-239">Příklad: `http://user01:password@proxy01.contoso.com:8080`</span><span class="sxs-lookup"><span data-stu-id="1a6c1-239">For example: `http://user01:password@proxy01.contoso.com:8080`</span></span>

<span data-ttu-id="1a6c1-240">Proxy server lze zadat během instalace nebo úpravou konfiguračního souboru proxy.conf po instalaci.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-240">The proxy server can be specified during installation or by modifying the proxy.conf configuration file after installation.</span></span>   

### <a name="specify-proxy-configuration-during-installation"></a><span data-ttu-id="1a6c1-241">Zadejte konfiguraci proxy serveru během instalace</span><span class="sxs-lookup"><span data-stu-id="1a6c1-241">Specify proxy configuration during installation</span></span>
<span data-ttu-id="1a6c1-242">`-p` Nebo `--proxy` argument pro instalaci sady omsagent Určuje konfiguraci proxy serveru používat.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-242">The `-p` or `--proxy` argument for the omsagent installation bundle specifies the proxy configuration to use.</span></span> 

```
sudo sh ./omsagent-<version>.universal.x64.sh --upgrade -p http://<proxy user>:<proxy password>@<proxy address>:<proxy port> -w <workspace id> -s <shared key>
```

### <a name="define-the-proxy-configuration-in-a-file"></a><span data-ttu-id="1a6c1-243">Zadejte konfiguraci proxy serveru v souboru</span><span class="sxs-lookup"><span data-stu-id="1a6c1-243">Define the proxy configuration in a file</span></span>
<span data-ttu-id="1a6c1-244">Konfiguraci proxy serveru můžete nastavit v souborech `/etc/opt/microsoft/omsagent/proxy.conf` a `/etc/opt/microsoft/omsagent/conf/proxy.conf `.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-244">The proxy configuration can be set in the files `/etc/opt/microsoft/omsagent/proxy.conf`  and `/etc/opt/microsoft/omsagent/conf/proxy.conf `.</span></span> <span data-ttu-id="1a6c1-245">Soubory mohou být přímo vytvořená nebo upravená, ale jejich oprávnění musí být aktualizovány k udělte že oprávnění na souborech čtení omiuser uživatele.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-245">The files can be directly created or edited, but their permissions must be updated to grant the omiuser user read permission on the files.</span></span> <span data-ttu-id="1a6c1-246">Například:</span><span class="sxs-lookup"><span data-stu-id="1a6c1-246">For example:</span></span>
```
proxyconf="https://proxyuser:proxypassword@proxyserver01:8080"
sudo echo $proxyconf >>/etc/opt/microsoft/omsagent/proxy.conf
sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/proxy.conf
sudo chmod 600 /etc/opt/microsoft/omsagent/proxy.conf /etc/opt/microsoft/omsagent/conf/proxy.conf  
sudo /opt/microsoft/omsagent/bin/service_control restart [<workspace id>]
```

### <a name="removing-the-proxy-configuration"></a><span data-ttu-id="1a6c1-247">Odebírá se konfigurace proxy serveru</span><span class="sxs-lookup"><span data-stu-id="1a6c1-247">Removing the proxy configuration</span></span>
<span data-ttu-id="1a6c1-248">Chcete-li odebrat konfiguraci dříve definovaném proxy a vrátit se do přímé připojení, odeberte soubor proxy.conf:</span><span class="sxs-lookup"><span data-stu-id="1a6c1-248">To remove a previously defined proxy configuration and revert to direct connectivity, remove the proxy.conf file:</span></span>
```
sudo rm /etc/opt/microsoft/omsagent/proxy.conf /etc/opt/microsoft/omsagent/conf/proxy.conf
sudo /opt/microsoft/omsagent/bin/service_control restart 
```

## <a name="onboarding-with-operations-management-suite"></a><span data-ttu-id="1a6c1-249">Registrace pomocí služby Operations Management Suite</span><span class="sxs-lookup"><span data-stu-id="1a6c1-249">Onboarding with Operations Management Suite</span></span>
<span data-ttu-id="1a6c1-250">Pokud ID a klíč nebyla zadána při instalaci sady, musí být agenta následně zaregistrován u služby Operations Management Suite.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-250">If a workspace ID and key were not provided during the bundle installation, the agent must be subsequently registered with Operations Management Suite.</span></span>

### <a name="onboarding-using-the-command-line"></a><span data-ttu-id="1a6c1-251">Registrace pomocí příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="1a6c1-251">Onboarding using the command line</span></span>
<span data-ttu-id="1a6c1-252">Spusťte příkaz omsadmin.sh se zadaným id pracovního prostoru a klíč vašeho pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-252">Run the omsadmin.sh command supplying the workspace id and key for your workspace.</span></span> <span data-ttu-id="1a6c1-253">Tento příkaz musí spustit jako kořenového adresáře (s zvýšení oprávnění sudo):</span><span class="sxs-lookup"><span data-stu-id="1a6c1-253">This command must be run as root (with sudo elevation):</span></span>
```
cd /opt/microsoft/omsagent/bin
sudo ./omsadmin.sh -w <WorkspaceID> -s <Shared Key>
```

### <a name="onboarding-using-a-file"></a><span data-ttu-id="1a6c1-254">Registrace pomocí souboru.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-254">Onboarding using a file</span></span>
1.  <span data-ttu-id="1a6c1-255">Vytvoření souboru `/etc/omsagent-onboard.conf`.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-255">Create the file `/etc/omsagent-onboard.conf`.</span></span> <span data-ttu-id="1a6c1-256">Soubor musí být přístupné pro čtení a zápis pro kořenový adresář.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-256">The file must be readable and writable for root.</span></span>
`sudo vi /etc/omsagent-onboard.conf`
2.  <span data-ttu-id="1a6c1-257">V souboru s ID pracovního prostoru a sdílený klíč, vložte následující řádky:</span><span class="sxs-lookup"><span data-stu-id="1a6c1-257">Insert the following lines in the file with your Workspace ID and Shared Key:</span></span>

        WORKSPACE_ID=<WorkspaceID>  
        SHARED_KEY=<Shared Key>  
   
3.  <span data-ttu-id="1a6c1-258">Spusťte následující příkaz k Onboard k OMS:`sudo /opt/microsoft/omsagent/bin/omsadmin.sh`</span><span class="sxs-lookup"><span data-stu-id="1a6c1-258">Run the following command to Onboard to OMS: `sudo /opt/microsoft/omsagent/bin/omsadmin.sh`</span></span>
4.  <span data-ttu-id="1a6c1-259">Soubor je odstraněn na úspěšnou registraci.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-259">The file is deleted on successful onboarding.</span></span>

## <a name="enable-the-oms-agent-for-linux-to-report-to-system-center-operations-manager"></a><span data-ttu-id="1a6c1-260">Povolte agenta OMS pro Linux tak, aby odesílaly System Center Operations Manager</span><span class="sxs-lookup"><span data-stu-id="1a6c1-260">Enable the OMS Agent for Linux to report to System Center Operations Manager</span></span>
<span data-ttu-id="1a6c1-261">Proveďte následující kroky konfigurace agenta OMS pro Linux informuje o skupině pro správu System Center Operations Manager.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-261">Perform the following steps to configure the OMS Agent for Linux to report to a System Center Operations Manager management group.</span></span>  

1. <span data-ttu-id="1a6c1-262">Upravte soubor`/etc/opt/omi/conf/omiserver.conf`</span><span class="sxs-lookup"><span data-stu-id="1a6c1-262">Edit the file `/etc/opt/omi/conf/omiserver.conf`</span></span>
2. <span data-ttu-id="1a6c1-263">Ujistěte se, že na začátek řádku s **httpsport =** definuje port 1270.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-263">Ensure that the line beginning with **httpsport=** defines the port 1270.</span></span> <span data-ttu-id="1a6c1-264">Například:`httpsport=1270`</span><span class="sxs-lookup"><span data-stu-id="1a6c1-264">Such as: `httpsport=1270`</span></span>
3. <span data-ttu-id="1a6c1-265">Restartujte OMI server:`sudo /opt/omi/bin/service_control restart`</span><span class="sxs-lookup"><span data-stu-id="1a6c1-265">Restart the OMI server: `sudo /opt/omi/bin/service_control restart`</span></span>

## <a name="agent-logs"></a><span data-ttu-id="1a6c1-266">Protokoly agenta</span><span class="sxs-lookup"><span data-stu-id="1a6c1-266">Agent logs</span></span>
<span data-ttu-id="1a6c1-267">Protokoly OMS agenta pro Linux najdete tady: `/var/opt/microsoft/omsagent/<workspace id>/log/` naleznete v protokolech programu omsconfig (Konfigurace agenta) na: `/var/opt/microsoft/omsconfig/log/` protokoly pro součásti infrastruktury OMI a SCX (které poskytují data metriky výkonu) najdete na:`/var/opt/omi/log/ and /var/opt/microsoft/scx/log`</span><span class="sxs-lookup"><span data-stu-id="1a6c1-267">The logs for the OMS Agent for Linux can be found at: `/var/opt/microsoft/omsagent/<workspace id>/log/` The logs for the omsconfig (agent configuration) program can be found at: `/var/opt/microsoft/omsconfig/log/` Logs for the OMI and SCX components (which provide performance metrics data) can be found at: `/var/opt/omi/log/ and /var/opt/microsoft/scx/log`</span></span>

### <a name="log-rotation-configuration"></a><span data-ttu-id="1a6c1-268">Protokol otočení konfigurace ##</span><span class="sxs-lookup"><span data-stu-id="1a6c1-268">Log rotation configuration##</span></span>
<span data-ttu-id="1a6c1-269">Konfigurace otáčení protokolu pro omsagent najdete tady:`/etc/logrotate.d/omsagent-<workspace id>`</span><span class="sxs-lookup"><span data-stu-id="1a6c1-269">The log rotate configuration for omsagent can be found at: `/etc/logrotate.d/omsagent-<workspace id>`</span></span>

<span data-ttu-id="1a6c1-270">Výchozí nastavení je:</span><span class="sxs-lookup"><span data-stu-id="1a6c1-270">The default settings are:</span></span> 
```
/var/opt/microsoft/omsagent/<workspace id>/log/omsagent.log {
    rotate 5
    missingok
    notifempty
    compress
    size 50k
    copytruncate
}
```

## <a name="uninstalling-the-oms-agent-for-linux"></a><span data-ttu-id="1a6c1-271">Odinstalování agenta OMS pro Linux</span><span class="sxs-lookup"><span data-stu-id="1a6c1-271">Uninstalling the OMS Agent for Linux</span></span>
<span data-ttu-id="1a6c1-272">Balíčky agenta odinstalovat spuštěním souboru .sh sady s `--purge` argument, který kompletně odebere agent a jeho konfigurace z počítače.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-272">The agent packages can be uninstalled by running the bundle .sh file with the `--purge` argument, which completely removes the agent and its configuration from the computer.</span></span>   

```
> sudo rpm -e omsconfig
> sudo rpm -e omsagent
> sudo /opt/microsoft/scx/bin/uninstall
```

## <a name="troubleshooting"></a><span data-ttu-id="1a6c1-273">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="1a6c1-273">Troubleshooting</span></span>

### <a name="issue-unable-to-connect-through-proxy-to-oms"></a><span data-ttu-id="1a6c1-274">Problém: Nelze se připojit prostřednictvím proxy serveru k OMS</span><span class="sxs-lookup"><span data-stu-id="1a6c1-274">Issue: Unable to connect through proxy to OMS</span></span>

#### <a name="probable-causes"></a><span data-ttu-id="1a6c1-275">Možných příčin</span><span class="sxs-lookup"><span data-stu-id="1a6c1-275">Probable causes</span></span>
* <span data-ttu-id="1a6c1-276">Zadaný během registrace proxy serveru byl nesprávný</span><span class="sxs-lookup"><span data-stu-id="1a6c1-276">The proxy specified during onboarding was incorrect</span></span>
* <span data-ttu-id="1a6c1-277">Koncové body služby OMS nejsou whitelistested ve vašem datovém centru</span><span class="sxs-lookup"><span data-stu-id="1a6c1-277">The OMS Service Endpoints are not whitelistested in your datacenter</span></span> 

#### <a name="resolutions"></a><span data-ttu-id="1a6c1-278">Řešení</span><span class="sxs-lookup"><span data-stu-id="1a6c1-278">Resolutions</span></span>
1. <span data-ttu-id="1a6c1-279">Reonboard ke službě OMS s agentem OMS pro Linux pomocí následujícího příkazu s parametrem `-v` povolena.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-279">Reonboard to the OMS Service with the OMS Agent for Linux by using the following command with the option `-v` enabled.</span></span> <span data-ttu-id="1a6c1-280">To umožňuje podrobný výstup agenta připojení prostřednictvím proxy serveru pro službu OMS.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-280">This allows verbose output of the agent connecting through the proxy to the OMS Service.</span></span> 
`/opt/microsoft/omsagent/bin/omsadmin.sh -w <OMS Workspace ID> -s <OMS Workspace Key> -p <Proxy Conf> -v`

2. <span data-ttu-id="1a6c1-281">Projděte si část [Konfigurace agenta pro použití s proxy serveru HTTP server(#configuring the-agent-for-use-with-a-http-proxy-server) ověření jste správně nakonfigurovali agenta pro komunikaci přes proxy server.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-281">Review the section [Configuring the agent for use with an HTTP proxy server(#configuring the-agent-for-use-with-a-http-proxy-server) to verify you have properly configured the agent to communicate through a proxy server.</span></span>    
* <span data-ttu-id="1a6c1-282">Překontrolujte, že vytvoření následujících koncových bodů služby OMS jsou seznam povolených adres:</span><span class="sxs-lookup"><span data-stu-id="1a6c1-282">Double check that the following OMS Service endpoints are whitelisted:</span></span>

    |<span data-ttu-id="1a6c1-283">Prostředek agenta</span><span class="sxs-lookup"><span data-stu-id="1a6c1-283">Agent Resource</span></span>| <span data-ttu-id="1a6c1-284">Porty</span><span class="sxs-lookup"><span data-stu-id="1a6c1-284">Ports</span></span> |  
    |------|---------|  
    |<span data-ttu-id="1a6c1-285">*.ods.opinsights.azure.com</span><span class="sxs-lookup"><span data-stu-id="1a6c1-285">*.ods.opinsights.azure.com</span></span> | <span data-ttu-id="1a6c1-286">Port 443</span><span class="sxs-lookup"><span data-stu-id="1a6c1-286">Port 443</span></span>|   
    |<span data-ttu-id="1a6c1-287">*.oms.opinsights.azure.com</span><span class="sxs-lookup"><span data-stu-id="1a6c1-287">*.oms.opinsights.azure.com</span></span> | <span data-ttu-id="1a6c1-288">Port 443</span><span class="sxs-lookup"><span data-stu-id="1a6c1-288">Port 443</span></span>|   
    |<span data-ttu-id="1a6c1-289">ods.systemcenteradvisor.com</span><span class="sxs-lookup"><span data-stu-id="1a6c1-289">ods.systemcenteradvisor.com</span></span> | <span data-ttu-id="1a6c1-290">Port 443</span><span class="sxs-lookup"><span data-stu-id="1a6c1-290">Port 443</span></span>|   
    |<span data-ttu-id="1a6c1-291">*.BLOB.Core.Windows.NET/</span><span class="sxs-lookup"><span data-stu-id="1a6c1-291">*.blob.core.windows.net/</span></span> | <span data-ttu-id="1a6c1-292">Port 443</span><span class="sxs-lookup"><span data-stu-id="1a6c1-292">Port 443</span></span>|   

### <a name="issue-you-receive-a-403-error-when-trying-to-onboard"></a><span data-ttu-id="1a6c1-293">Problém: Obdržíte 403 chybu při pokusu o zařadit do provozu</span><span class="sxs-lookup"><span data-stu-id="1a6c1-293">Issue: You receive a 403 error when trying to onboard</span></span>

#### <a name="probable-causes"></a><span data-ttu-id="1a6c1-294">Možných příčin</span><span class="sxs-lookup"><span data-stu-id="1a6c1-294">Probable causes</span></span>
* <span data-ttu-id="1a6c1-295">Datum a čas je nesprávný na Linux Server</span><span class="sxs-lookup"><span data-stu-id="1a6c1-295">Date and Time is incorrect on Linux Server</span></span> 
* <span data-ttu-id="1a6c1-296">ID pracovního prostoru a klíč pracovního prostoru používá nejsou správné</span><span class="sxs-lookup"><span data-stu-id="1a6c1-296">Workspace ID and Workspace Key used are not correct</span></span>

#### <a name="resolution"></a><span data-ttu-id="1a6c1-297">Řešení</span><span class="sxs-lookup"><span data-stu-id="1a6c1-297">Resolution</span></span>

1. <span data-ttu-id="1a6c1-298">Zkontrolujte čas na serveru Linux s datem příkaz.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-298">Check the time on your Linux server with the command date.</span></span> <span data-ttu-id="1a6c1-299">Pokud je doba +/-15 minut od aktuálního času, registrace se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-299">If the time is +/- 15 minutes from current time, then onboarding fails.</span></span> <span data-ttu-id="1a6c1-300">Správné to aktualizujte datum a časové pásmo serveru Linux.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-300">To correct this update the date and/or timezone of your Linux server.</span></span> 
2. <span data-ttu-id="1a6c1-301">Ověřte, jestli že je nainstalovaná nejnovější verze agenta OMS pro Linux.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-301">Verify you have installed the latest version of the OMS Agent for Linux.</span></span>  <span data-ttu-id="1a6c1-302">Nejnovější verze nyní upozorní, že jste Pokud časového posunu způsobuje selhání registrace.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-302">The newest version now notifies you if time skew is causing the onboarding failure.</span></span>
3. <span data-ttu-id="1a6c1-303">Reonboard pomocí správné ID pracovního prostoru a klíč pracovního prostoru pokynů instalace dříve v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-303">Reonboard using correct Workspace ID and Workspace Key following the installation instructions earlier in this topic.</span></span>

### <a name="issue-you-see-a-500-and-404-error-in-the-log-file-right-after-onboarding"></a><span data-ttu-id="1a6c1-304">Problém: Zobrazí 500 a 404 Chyba v souboru protokolu hned po registraci</span><span class="sxs-lookup"><span data-stu-id="1a6c1-304">Issue: You see a 500 and 404 error in the log file right after onboarding</span></span>
<span data-ttu-id="1a6c1-305">Jde o známý problém, ke kterému dochází na první nahrávání dat Linux do pracovního prostoru OMS.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-305">This is a known issue that occurs on first upload of Linux data into an OMS workspace.</span></span> <span data-ttu-id="1a6c1-306">To nemá vliv dat probíhá odeslané nebo služby.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-306">This does not affect data being sent or service experience.</span></span>

### <a name="issue--you-are-not-seeing-any-data-in-the-oms-portal"></a><span data-ttu-id="1a6c1-307">Problém: Nevidíte všechna data na portálu OMS</span><span class="sxs-lookup"><span data-stu-id="1a6c1-307">Issue:  You are not seeing any data in the OMS portal</span></span>

#### <a name="probable-causes"></a><span data-ttu-id="1a6c1-308">Možných příčin</span><span class="sxs-lookup"><span data-stu-id="1a6c1-308">Probable causes</span></span>

- <span data-ttu-id="1a6c1-309">Připojování ke službě OMS se nezdařilo</span><span class="sxs-lookup"><span data-stu-id="1a6c1-309">Onboarding to the OMS Service failed</span></span>
- <span data-ttu-id="1a6c1-310">Připojení ke službě OMS blokováno.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-310">Connection to the OMS Service is blocked</span></span>
- <span data-ttu-id="1a6c1-311">Vytvoření zálohy OMS agenta pro Linux data</span><span class="sxs-lookup"><span data-stu-id="1a6c1-311">OMS Agent for Linux data is backed up</span></span>

#### <a name="resolutions"></a><span data-ttu-id="1a6c1-312">Řešení</span><span class="sxs-lookup"><span data-stu-id="1a6c1-312">Resolutions</span></span>
1. <span data-ttu-id="1a6c1-313">Zkontrolujte, pokud registrace služby OMS po úspěšné kontrole, jestli existuje následující soubor:`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf`</span><span class="sxs-lookup"><span data-stu-id="1a6c1-313">Check if onboarding the OMS Service was successful by checking if the following file exists: `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf`</span></span>
2. <span data-ttu-id="1a6c1-314">Pomocí Reonboard `omsadmin.sh` příkazového řádku pokyny</span><span class="sxs-lookup"><span data-stu-id="1a6c1-314">Reonboard using the `omsadmin.sh` command-line instructions</span></span>
3. <span data-ttu-id="1a6c1-315">Pokud používáte proxy server, podívejte se na proxy řešení kroků uvedených výše.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-315">If using a proxy, refer to the proxy resolution steps provided earlier.</span></span>
4. <span data-ttu-id="1a6c1-316">V některých případech při OMS agenta pro Linux nemůže komunikovat se službou OMS data na agentovi je zařazených do fronty pro velikost vyrovnávací paměti úplná, což je 50 MB.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-316">In some cases, when the OMS Agent for Linux cannot communicate with the OMS Service, data on the agent is queued to the full buffer size, which is 50 MB.</span></span> <span data-ttu-id="1a6c1-317">Spuštěním následujícího příkazu by měla být restartována OMS agenta pro Linux: `/opt/microsoft/omsagent/bin/service_control restart [<workspace id>]`.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-317">The OMS Agent for Linux should be restarted by running the following command: `/opt/microsoft/omsagent/bin/service_control restart [<workspace id>]`.</span></span> 

    >[!NOTE]
    ><span data-ttu-id="1a6c1-318">Tento problém vyřešen v 1.1.0-28 verze agenta nebo novější.</span><span class="sxs-lookup"><span data-stu-id="1a6c1-318">This issue is fixed in agent version 1.1.0-28 and later.</span></span>
> 