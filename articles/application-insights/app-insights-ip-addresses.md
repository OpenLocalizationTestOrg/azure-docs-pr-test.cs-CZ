---
title: "aaaIP adresy používané při Application Insights | Microsoft Docs"
description: "Výjimky brány firewall serveru vyžadují Application Insights"
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: 44d989f8-bae9-40ff-bfd5-8343d3e59358
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 8/11/2017
ms.author: bwren
ms.openlocfilehash: 2c101b8da2ba9594fbff607f4f7551cda80c3c25
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="ip-addresses-used-by-application-insights"></a><span data-ttu-id="a3fa0-103">IP adresy používané službou Application Insights</span><span class="sxs-lookup"><span data-stu-id="a3fa0-103">IP addresses used by Application Insights</span></span>
<span data-ttu-id="a3fa0-104">Hello [Azure Application Insights](app-insights-overview.md) služba používá počet IP adres.</span><span class="sxs-lookup"><span data-stu-id="a3fa0-104">hello [Azure Application Insights](app-insights-overview.md) service uses a number of IP addresses.</span></span> <span data-ttu-id="a3fa0-105">Tooknow bude pravděpodobně nutné tyto adresy, pokud hello aplikaci, která se nachází za bránou firewall.</span><span class="sxs-lookup"><span data-stu-id="a3fa0-105">You might need tooknow these addresses if hello app that you are monitoring is hosted behind a firewall.</span></span>

> [!NOTE]
> <span data-ttu-id="a3fa0-106">I když tyto adresy jsou statické, je možné, že je nutné zadat toochange jim tootime čas.</span><span class="sxs-lookup"><span data-stu-id="a3fa0-106">Although these addresses are static, it's possible that we will need toochange them from time tootime.</span></span>
> 
> 

## <a name="outgoing-ports"></a><span data-ttu-id="a3fa0-107">Odchozí porty</span><span class="sxs-lookup"><span data-stu-id="a3fa0-107">Outgoing ports</span></span>
<span data-ttu-id="a3fa0-108">Je třeba tooopen některé Odchozí porty v váš server brány firewall tooallow hello Application Insights SDK nebo monitorování stavu toosend data toohello portálu:</span><span class="sxs-lookup"><span data-stu-id="a3fa0-108">You need tooopen some outgoing ports in your server's firewall tooallow hello Application Insights SDK and/or Status Monitor toosend data toohello portal:</span></span>

| <span data-ttu-id="a3fa0-109">Účel</span><span class="sxs-lookup"><span data-stu-id="a3fa0-109">Purpose</span></span> | <span data-ttu-id="a3fa0-110">ADRESA URL</span><span class="sxs-lookup"><span data-stu-id="a3fa0-110">URL</span></span> | <span data-ttu-id="a3fa0-111">IP adresa</span><span class="sxs-lookup"><span data-stu-id="a3fa0-111">IP</span></span> | <span data-ttu-id="a3fa0-112">Porty</span><span class="sxs-lookup"><span data-stu-id="a3fa0-112">Ports</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="a3fa0-113">Telemetrická data</span><span class="sxs-lookup"><span data-stu-id="a3fa0-113">Telemetry</span></span> |<span data-ttu-id="a3fa0-114">adresu DC.Services.VisualStudio.com</span><span class="sxs-lookup"><span data-stu-id="a3fa0-114">dc.services.visualstudio.com</span></span><br/><span data-ttu-id="a3fa0-115">DC.applicationinsights.microsoft.com</span><span class="sxs-lookup"><span data-stu-id="a3fa0-115">dc.applicationinsights.microsoft.com</span></span> |<span data-ttu-id="a3fa0-116">40.114.241.141</span><span class="sxs-lookup"><span data-stu-id="a3fa0-116">40.114.241.141</span></span><br/><span data-ttu-id="a3fa0-117">104.45.136.42</span><span class="sxs-lookup"><span data-stu-id="a3fa0-117">104.45.136.42</span></span><br/><span data-ttu-id="a3fa0-118">40.84.189.107</span><span class="sxs-lookup"><span data-stu-id="a3fa0-118">40.84.189.107</span></span><br/><span data-ttu-id="a3fa0-119">168.63.242.221</span><span class="sxs-lookup"><span data-stu-id="a3fa0-119">168.63.242.221</span></span><br/><span data-ttu-id="a3fa0-120">52.167.221.184</span><span class="sxs-lookup"><span data-stu-id="a3fa0-120">52.167.221.184</span></span><br/><span data-ttu-id="a3fa0-121">52.169.64.244</span><span class="sxs-lookup"><span data-stu-id="a3fa0-121">52.169.64.244</span></span> |<span data-ttu-id="a3fa0-122">443</span><span class="sxs-lookup"><span data-stu-id="a3fa0-122">443</span></span> |
| <span data-ttu-id="a3fa0-123">Živý datový proud metriky</span><span class="sxs-lookup"><span data-stu-id="a3fa0-123">Live Metrics Stream</span></span> |<span data-ttu-id="a3fa0-124">RT.Services.VisualStudio.com</span><span class="sxs-lookup"><span data-stu-id="a3fa0-124">rt.services.visualstudio.com</span></span><br/><span data-ttu-id="a3fa0-125">RT.applicationinsights.microsoft.com</span><span class="sxs-lookup"><span data-stu-id="a3fa0-125">rt.applicationinsights.microsoft.com</span></span> |<span data-ttu-id="a3fa0-126">23.96.28.38</span><span class="sxs-lookup"><span data-stu-id="a3fa0-126">23.96.28.38</span></span><br/><span data-ttu-id="a3fa0-127">13.92.40.198</span><span class="sxs-lookup"><span data-stu-id="a3fa0-127">13.92.40.198</span></span> |<span data-ttu-id="a3fa0-128">443</span><span class="sxs-lookup"><span data-stu-id="a3fa0-128">443</span></span> |
| <span data-ttu-id="a3fa0-129">Interní Telemetrie</span><span class="sxs-lookup"><span data-stu-id="a3fa0-129">Internal Telemetry</span></span> |<span data-ttu-id="a3fa0-130">breeze.aimon.applicationinsights.IO</span><span class="sxs-lookup"><span data-stu-id="a3fa0-130">breeze.aimon.applicationinsights.io</span></span> |<span data-ttu-id="a3fa0-131">52.161.11.71</span><span class="sxs-lookup"><span data-stu-id="a3fa0-131">52.161.11.71</span></span> |<span data-ttu-id="a3fa0-132">443</span><span class="sxs-lookup"><span data-stu-id="a3fa0-132">443</span></span> |

## <a name="status-monitor"></a><span data-ttu-id="a3fa0-133">Monitorování stavu</span><span class="sxs-lookup"><span data-stu-id="a3fa0-133">Status Monitor</span></span>
<span data-ttu-id="a3fa0-134">Stav monitorování konfigurace – je potřeba pouze při provádění změn.</span><span class="sxs-lookup"><span data-stu-id="a3fa0-134">Status Monitor Configuration - needed only when making changes.</span></span>

| <span data-ttu-id="a3fa0-135">Účel</span><span class="sxs-lookup"><span data-stu-id="a3fa0-135">Purpose</span></span> | <span data-ttu-id="a3fa0-136">ADRESA URL</span><span class="sxs-lookup"><span data-stu-id="a3fa0-136">URL</span></span> | <span data-ttu-id="a3fa0-137">IP adresa</span><span class="sxs-lookup"><span data-stu-id="a3fa0-137">IP</span></span> | <span data-ttu-id="a3fa0-138">Porty</span><span class="sxs-lookup"><span data-stu-id="a3fa0-138">Ports</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="a3fa0-139">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="a3fa0-139">Configuration</span></span> |`management.core.windows.net` | |`443` |
| <span data-ttu-id="a3fa0-140">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="a3fa0-140">Configuration</span></span> |`management.azure.com` | |`443` |
| <span data-ttu-id="a3fa0-141">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="a3fa0-141">Configuration</span></span> |`login.windows.net` | |`443` |
| <span data-ttu-id="a3fa0-142">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="a3fa0-142">Configuration</span></span> |`login.microsoftonline.com` | |`443` |
| <span data-ttu-id="a3fa0-143">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="a3fa0-143">Configuration</span></span> |`secure.aadcdn.microsoftonline-p.com` | |`443` |
| <span data-ttu-id="a3fa0-144">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="a3fa0-144">Configuration</span></span> |`auth.gfx.ms` | |`443` |
| <span data-ttu-id="a3fa0-145">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="a3fa0-145">Configuration</span></span> |`login.live.com` | |`443` |
| <span data-ttu-id="a3fa0-146">Instalace</span><span class="sxs-lookup"><span data-stu-id="a3fa0-146">Installation</span></span> |`packages.nuget.org` | |`443` |

## <a name="hockeyapp"></a><span data-ttu-id="a3fa0-147">HockeyApp</span><span class="sxs-lookup"><span data-stu-id="a3fa0-147">HockeyApp</span></span>
| <span data-ttu-id="a3fa0-148">Účel</span><span class="sxs-lookup"><span data-stu-id="a3fa0-148">Purpose</span></span> | <span data-ttu-id="a3fa0-149">ADRESA URL</span><span class="sxs-lookup"><span data-stu-id="a3fa0-149">URL</span></span> | <span data-ttu-id="a3fa0-150">IP adresa</span><span class="sxs-lookup"><span data-stu-id="a3fa0-150">IP</span></span> | <span data-ttu-id="a3fa0-151">Porty</span><span class="sxs-lookup"><span data-stu-id="a3fa0-151">Ports</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="a3fa0-152">Data o chybách</span><span class="sxs-lookup"><span data-stu-id="a3fa0-152">Crash data</span></span> |<span data-ttu-id="a3fa0-153">Gate.hockeyapp.NET</span><span class="sxs-lookup"><span data-stu-id="a3fa0-153">gate.hockeyapp.net</span></span> |<span data-ttu-id="a3fa0-154">104.45.136.42</span><span class="sxs-lookup"><span data-stu-id="a3fa0-154">104.45.136.42</span></span> |<span data-ttu-id="a3fa0-155">80, 443</span><span class="sxs-lookup"><span data-stu-id="a3fa0-155">80, 443</span></span> |

## <a name="availability-tests"></a><span data-ttu-id="a3fa0-156">Testy dostupnosti</span><span class="sxs-lookup"><span data-stu-id="a3fa0-156">Availability tests</span></span>
<span data-ttu-id="a3fa0-157">Toto je seznam hello adres, ze kterého [testy dostupnosti webu](app-insights-monitor-web-app-availability.md) spouštějí.</span><span class="sxs-lookup"><span data-stu-id="a3fa0-157">This is hello list of addresses from which [availability web tests](app-insights-monitor-web-app-availability.md) are run.</span></span> <span data-ttu-id="a3fa0-158">Pokud chcete, aby toorun webové testy v aplikaci, ale váš webový server je omezená tooserving konkrétní klientů, pak bude mít toopermit příchozí provoz z našich serverech test dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="a3fa0-158">If you want toorun web tests on your app, but your web server is restricted tooserving specific clients, then you will have toopermit incoming traffic from our availability test servers.</span></span>

<span data-ttu-id="a3fa0-159">Otevřete porty 80 (http) a 443 (https) pro příchozí provoz z těchto adres (IP adresy jsou seskupené podle umístění):</span><span class="sxs-lookup"><span data-stu-id="a3fa0-159">Open ports 80 (http) and 443 (https) for incoming traffic from these addresses (IP addresses are grouped by location):</span></span>

```
AU : Sydney
13.70.83.252
13.75.150.96
13.75.153.9
13.75.158.185
BR : Sao Paulo
191.232.32.122
191.232.172.45
191.232.176.218
191.232.191.225
CH : Zurich
94.245.66.43
94.245.66.44
94.245.66.45
94.245.66.48
FR : Paris
94.245.72.44
94.245.72.45
94.245.72.46
94.245.72.49
94.245.72.52
94.245.72.53
HK : Hong Kong
13.75.121.122
23.99.115.153
23.99.123.38
23.102.232.186
52.175.38.49
52.175.39.103
IE : Dublin
13.74.184.101
13.74.185.160
40.69.200.198
52.164.224.46
52.169.12.203
52.169.14.11
52.169.237.149
52.178.183.105
JP : Kawaguchi
52.243.33.33
52.243.33.141
52.243.35.253
52.243.41.117
NL : Amsterdam
52.174.166.113
52.174.178.96
52.174.31.140
52.174.35.14
52.178.104.23
52.178.109.190
52.178.111.139
52.233.166.221
RU : Moscow
94.245.82.32
94.245.82.33
94.245.82.37
94.245.82.38
SE : Stockholm
94.245.78.40
94.245.78.41
94.245.78.42
94.245.78.45
SG : Singapore
52.187.29.7
52.187.179.17
52.187.76.248
52.187.43.24
52.163.57.91
52.187.30.120
US : CA-San Jose
104.45.228.236
104.45.237.251
13.64.152.110
13.64.156.54
13.64.232.251
13.64.236.105
13.91.94.59
40.118.131.182
40.83.189.192
40.83.215.122
US : FL-Miami
65.54.78.49
65.54.78.50
65.54.78.51
65.54.78.54
65.54.78.57
65.54.78.58
65.54.78.59
65.54.78.60
US : IL-Chicago
23.96.247.139
23.96.249.113
52.162.124.242
52.162.126.139
52.162.241.147
52.162.246.222
52.162.247.136
52.237.153.231
52.237.154.216
52.237.156.14
52.237.157.218
52.237.157.37
US : TX-San Antonio
104.210.145.106
13.84.176.24
13.84.49.16
13.85.11.137
13.85.26.248
13.85.69.145
52.171.136.162
52.171.141.253
52.171.57.172
52.171.58.140
US : VA-Ashburn
13.82.218.95
13.90.96.71
13.90.98.52
13.92.137.70
40.85.187.235
40.87.61.61
52.168.8.247
52.170.38.79
52.170.80.61
52.179.9.26

```  

## <a name="data-access-api"></a><span data-ttu-id="a3fa0-160">Rozhraní API pro přístup k datům</span><span class="sxs-lookup"><span data-stu-id="a3fa0-160">Data access API</span></span>
| <span data-ttu-id="a3fa0-161">Účel</span><span class="sxs-lookup"><span data-stu-id="a3fa0-161">Purpose</span></span> | <span data-ttu-id="a3fa0-162">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="a3fa0-162">URI</span></span> | <span data-ttu-id="a3fa0-163">IP adresa</span><span class="sxs-lookup"><span data-stu-id="a3fa0-163">IP</span></span> | <span data-ttu-id="a3fa0-164">Porty</span><span class="sxs-lookup"><span data-stu-id="a3fa0-164">Ports</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="a3fa0-165">Rozhraní API</span><span class="sxs-lookup"><span data-stu-id="a3fa0-165">API</span></span> |<span data-ttu-id="a3fa0-166">API.applicationinsights.IO</span><span class="sxs-lookup"><span data-stu-id="a3fa0-166">api.applicationinsights.io</span></span><br/><span data-ttu-id="a3fa0-167">api1.applicationinsights.IO</span><span class="sxs-lookup"><span data-stu-id="a3fa0-167">api1.applicationinsights.io</span></span><br/><span data-ttu-id="a3fa0-168">api2.applicationinsights.IO</span><span class="sxs-lookup"><span data-stu-id="a3fa0-168">api2.applicationinsights.io</span></span><br/><span data-ttu-id="a3fa0-169">api3.applicationinsights.IO</span><span class="sxs-lookup"><span data-stu-id="a3fa0-169">api3.applicationinsights.io</span></span><br/><span data-ttu-id="a3fa0-170">api4.applicationinsights.IO</span><span class="sxs-lookup"><span data-stu-id="a3fa0-170">api4.applicationinsights.io</span></span><br/><span data-ttu-id="a3fa0-171">api5.applicationinsights.IO</span><span class="sxs-lookup"><span data-stu-id="a3fa0-171">api5.applicationinsights.io</span></span> |<span data-ttu-id="a3fa0-172">13.82.26.252</span><span class="sxs-lookup"><span data-stu-id="a3fa0-172">13.82.26.252</span></span><br/><span data-ttu-id="a3fa0-173">40.76.213.73</span><span class="sxs-lookup"><span data-stu-id="a3fa0-173">40.76.213.73</span></span> |<span data-ttu-id="a3fa0-174">80,443</span><span class="sxs-lookup"><span data-stu-id="a3fa0-174">80,443</span></span> |
| <span data-ttu-id="a3fa0-175">Dokumentace rozhraní API</span><span class="sxs-lookup"><span data-stu-id="a3fa0-175">API docs</span></span> |<span data-ttu-id="a3fa0-176">dev.applicationinsights.IO</span><span class="sxs-lookup"><span data-stu-id="a3fa0-176">dev.applicationinsights.io</span></span><br/><span data-ttu-id="a3fa0-177">dev.applicationinsights.microsoft.com</span><span class="sxs-lookup"><span data-stu-id="a3fa0-177">dev.applicationinsights.microsoft.com</span></span><br/><span data-ttu-id="a3fa0-178">dev.aisvc.VisualStudio.com</span><span class="sxs-lookup"><span data-stu-id="a3fa0-178">dev.aisvc.visualstudio.com</span></span><br/><span data-ttu-id="a3fa0-179">www.applicationinsights.IO</span><span class="sxs-lookup"><span data-stu-id="a3fa0-179">www.applicationinsights.io</span></span><br/><span data-ttu-id="a3fa0-180">www.applicationinsights.microsoft.com</span><span class="sxs-lookup"><span data-stu-id="a3fa0-180">www.applicationinsights.microsoft.com</span></span><br/><span data-ttu-id="a3fa0-181">www.aisvc.VisualStudio.com</span><span class="sxs-lookup"><span data-stu-id="a3fa0-181">www.aisvc.visualstudio.com</span></span> |<span data-ttu-id="a3fa0-182">13.82.24.149</span><span class="sxs-lookup"><span data-stu-id="a3fa0-182">13.82.24.149</span></span><br/><span data-ttu-id="a3fa0-183">40.114.82.10</span><span class="sxs-lookup"><span data-stu-id="a3fa0-183">40.114.82.10</span></span> |<span data-ttu-id="a3fa0-184">80,443</span><span class="sxs-lookup"><span data-stu-id="a3fa0-184">80,443</span></span> |
| <span data-ttu-id="a3fa0-185">Vnitřní rozhraní API</span><span class="sxs-lookup"><span data-stu-id="a3fa0-185">Internal API</span></span> |<span data-ttu-id="a3fa0-186">aigs.aisvc.VisualStudio.com</span><span class="sxs-lookup"><span data-stu-id="a3fa0-186">aigs.aisvc.visualstudio.com</span></span><br/><span data-ttu-id="a3fa0-187">aigs1.aisvc.VisualStudio.com</span><span class="sxs-lookup"><span data-stu-id="a3fa0-187">aigs1.aisvc.visualstudio.com</span></span><br/><span data-ttu-id="a3fa0-188">aigs2.aisvc.VisualStudio.com</span><span class="sxs-lookup"><span data-stu-id="a3fa0-188">aigs2.aisvc.visualstudio.com</span></span><br/><span data-ttu-id="a3fa0-189">aigs3.aisvc.VisualStudio.com</span><span class="sxs-lookup"><span data-stu-id="a3fa0-189">aigs3.aisvc.visualstudio.com</span></span><br/><span data-ttu-id="a3fa0-190">aigs4.aisvc.VisualStudio.com</span><span class="sxs-lookup"><span data-stu-id="a3fa0-190">aigs4.aisvc.visualstudio.com</span></span><br/><span data-ttu-id="a3fa0-191">aigs5.aisvc.VisualStudio.com</span><span class="sxs-lookup"><span data-stu-id="a3fa0-191">aigs5.aisvc.visualstudio.com</span></span><br/><span data-ttu-id="a3fa0-192">aigs6.aisvc.VisualStudio.com</span><span class="sxs-lookup"><span data-stu-id="a3fa0-192">aigs6.aisvc.visualstudio.com</span></span> |<span data-ttu-id="a3fa0-193">dynamické</span><span class="sxs-lookup"><span data-stu-id="a3fa0-193">dynamic</span></span>|<span data-ttu-id="a3fa0-194">443</span><span class="sxs-lookup"><span data-stu-id="a3fa0-194">443</span></span> |

## <a name="application-insights-analytics"></a><span data-ttu-id="a3fa0-195">Analýza statistiky aplikace</span><span class="sxs-lookup"><span data-stu-id="a3fa0-195">Application Insights Analytics</span></span>

| <span data-ttu-id="a3fa0-196">Účel</span><span class="sxs-lookup"><span data-stu-id="a3fa0-196">Purpose</span></span> | <span data-ttu-id="a3fa0-197">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="a3fa0-197">URI</span></span> | <span data-ttu-id="a3fa0-198">IP adresa</span><span class="sxs-lookup"><span data-stu-id="a3fa0-198">IP</span></span> | <span data-ttu-id="a3fa0-199">Porty</span><span class="sxs-lookup"><span data-stu-id="a3fa0-199">Ports</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="a3fa0-200">Portálu analýza</span><span class="sxs-lookup"><span data-stu-id="a3fa0-200">Analytics Portal</span></span> | <span data-ttu-id="a3fa0-201">Analytics.applicationinsights.IO</span><span class="sxs-lookup"><span data-stu-id="a3fa0-201">analytics.applicationinsights.io</span></span> | <span data-ttu-id="a3fa0-202">dynamické</span><span class="sxs-lookup"><span data-stu-id="a3fa0-202">dynamic</span></span> | <span data-ttu-id="a3fa0-203">80,443</span><span class="sxs-lookup"><span data-stu-id="a3fa0-203">80,443</span></span> |
| <span data-ttu-id="a3fa0-204">CDN</span><span class="sxs-lookup"><span data-stu-id="a3fa0-204">CDN</span></span> | <span data-ttu-id="a3fa0-205">applicationanalytics.azureedge.NET</span><span class="sxs-lookup"><span data-stu-id="a3fa0-205">applicationanalytics.azureedge.net</span></span> | <span data-ttu-id="a3fa0-206">dynamické</span><span class="sxs-lookup"><span data-stu-id="a3fa0-206">dynamic</span></span> | <span data-ttu-id="a3fa0-207">80,443</span><span class="sxs-lookup"><span data-stu-id="a3fa0-207">80,443</span></span> |
| <span data-ttu-id="a3fa0-208">Média CDN</span><span class="sxs-lookup"><span data-stu-id="a3fa0-208">Media CDN</span></span> | <span data-ttu-id="a3fa0-209">applicationanalyticsmedia.azureedge.NET</span><span class="sxs-lookup"><span data-stu-id="a3fa0-209">applicationanalyticsmedia.azureedge.net</span></span> | <span data-ttu-id="a3fa0-210">dynamické</span><span class="sxs-lookup"><span data-stu-id="a3fa0-210">dynamic</span></span> | <span data-ttu-id="a3fa0-211">80,443</span><span class="sxs-lookup"><span data-stu-id="a3fa0-211">80,443</span></span> |

<span data-ttu-id="a3fa0-212">Poznámka: *. vlastní domény applicationinsights.io týmem Application Insights.</span><span class="sxs-lookup"><span data-stu-id="a3fa0-212">Note: *.applicationinsights.io domain is owned by Application Insights team.</span></span>

## <a name="application-insights-azure-portal-extension"></a><span data-ttu-id="a3fa0-213">Application Insights rozšíření portálu Azure</span><span class="sxs-lookup"><span data-stu-id="a3fa0-213">Application Insights Azure Portal Extension</span></span>

| <span data-ttu-id="a3fa0-214">Účel</span><span class="sxs-lookup"><span data-stu-id="a3fa0-214">Purpose</span></span> | <span data-ttu-id="a3fa0-215">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="a3fa0-215">URI</span></span> | <span data-ttu-id="a3fa0-216">IP adresa</span><span class="sxs-lookup"><span data-stu-id="a3fa0-216">IP</span></span> | <span data-ttu-id="a3fa0-217">Porty</span><span class="sxs-lookup"><span data-stu-id="a3fa0-217">Ports</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="a3fa0-218">Rozšíření Application Insights</span><span class="sxs-lookup"><span data-stu-id="a3fa0-218">Application Insights Extension</span></span> | <span data-ttu-id="a3fa0-219">stamp2.app.insightsportal.VisualStudio.com</span><span class="sxs-lookup"><span data-stu-id="a3fa0-219">stamp2.app.insightsportal.visualstudio.com</span></span> | <span data-ttu-id="a3fa0-220">dynamické</span><span class="sxs-lookup"><span data-stu-id="a3fa0-220">dynamic</span></span> | <span data-ttu-id="a3fa0-221">80,443</span><span class="sxs-lookup"><span data-stu-id="a3fa0-221">80,443</span></span> |
| <span data-ttu-id="a3fa0-222">Application Insights rozšíření CDN</span><span class="sxs-lookup"><span data-stu-id="a3fa0-222">Application Insights Extension CDN</span></span> | <span data-ttu-id="a3fa0-223">insightsportal. prod2 cdn.aisvc.visualstudio.com</span><span class="sxs-lookup"><span data-stu-id="a3fa0-223">insightsportal-prod2-cdn.aisvc.visualstudio.com</span></span><br/><span data-ttu-id="a3fa0-224">insightsportal prod2 asiae cdn.aisvc.visualstudio.com</span><span class="sxs-lookup"><span data-stu-id="a3fa0-224">insightsportal-prod2-asiae-cdn.aisvc.visualstudio.com</span></span><br/><span data-ttu-id="a3fa0-225">insightsportal-cdn-aimon.applicationinsights.io</span><span class="sxs-lookup"><span data-stu-id="a3fa0-225">insightsportal-cdn-aimon.applicationinsights.io</span></span> | <span data-ttu-id="a3fa0-226">dynamické</span><span class="sxs-lookup"><span data-stu-id="a3fa0-226">dynamic</span></span> | <span data-ttu-id="a3fa0-227">80,443</span><span class="sxs-lookup"><span data-stu-id="a3fa0-227">80,443</span></span> |

## <a name="application-insights-sdks"></a><span data-ttu-id="a3fa0-228">Sadách Application Insights SDK</span><span class="sxs-lookup"><span data-stu-id="a3fa0-228">Application Insights SDKs</span></span>

| <span data-ttu-id="a3fa0-229">Účel</span><span class="sxs-lookup"><span data-stu-id="a3fa0-229">Purpose</span></span> | <span data-ttu-id="a3fa0-230">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="a3fa0-230">URI</span></span> | <span data-ttu-id="a3fa0-231">IP adresa</span><span class="sxs-lookup"><span data-stu-id="a3fa0-231">IP</span></span> | <span data-ttu-id="a3fa0-232">Porty</span><span class="sxs-lookup"><span data-stu-id="a3fa0-232">Ports</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="a3fa0-233">Application Insights JS SDK CDN</span><span class="sxs-lookup"><span data-stu-id="a3fa0-233">Application Insights JS SDK CDN</span></span> | <span data-ttu-id="a3fa0-234">az416426.vo.msecnd.NET</span><span class="sxs-lookup"><span data-stu-id="a3fa0-234">az416426.vo.msecnd.net</span></span> | <span data-ttu-id="a3fa0-235">dynamické</span><span class="sxs-lookup"><span data-stu-id="a3fa0-235">dynamic</span></span> | <span data-ttu-id="a3fa0-236">80,443</span><span class="sxs-lookup"><span data-stu-id="a3fa0-236">80,443</span></span> |
| <span data-ttu-id="a3fa0-237">Application Insights Java SDK</span><span class="sxs-lookup"><span data-stu-id="a3fa0-237">Application Insights Java SDK</span></span> | <span data-ttu-id="a3fa0-238">aijavasdk.BLOB.Core.Windows.NET</span><span class="sxs-lookup"><span data-stu-id="a3fa0-238">aijavasdk.blob.core.windows.net</span></span> | <span data-ttu-id="a3fa0-239">dynamické</span><span class="sxs-lookup"><span data-stu-id="a3fa0-239">dynamic</span></span> | <span data-ttu-id="a3fa0-240">80,443</span><span class="sxs-lookup"><span data-stu-id="a3fa0-240">80,443</span></span> |

## <a name="profiler"></a><span data-ttu-id="a3fa0-241">Profiler</span><span class="sxs-lookup"><span data-stu-id="a3fa0-241">Profiler</span></span>

| <span data-ttu-id="a3fa0-242">Účel</span><span class="sxs-lookup"><span data-stu-id="a3fa0-242">Purpose</span></span> | <span data-ttu-id="a3fa0-243">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="a3fa0-243">URI</span></span> | <span data-ttu-id="a3fa0-244">IP adresa</span><span class="sxs-lookup"><span data-stu-id="a3fa0-244">IP</span></span> | <span data-ttu-id="a3fa0-245">Porty</span><span class="sxs-lookup"><span data-stu-id="a3fa0-245">Ports</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="a3fa0-246">Agent</span><span class="sxs-lookup"><span data-stu-id="a3fa0-246">Agent</span></span> | <span data-ttu-id="a3fa0-247">Agent.azureserviceprofiler.NET</span><span class="sxs-lookup"><span data-stu-id="a3fa0-247">agent.azureserviceprofiler.net</span></span><br/><span data-ttu-id="a3fa0-248">*. agent.azureserviceprofiler.net</span><span class="sxs-lookup"><span data-stu-id="a3fa0-248">*.agent.azureserviceprofiler.net</span></span> | <span data-ttu-id="a3fa0-249">dynamické</span><span class="sxs-lookup"><span data-stu-id="a3fa0-249">dynamic</span></span> | <span data-ttu-id="a3fa0-250">443</span><span class="sxs-lookup"><span data-stu-id="a3fa0-250">443</span></span>
| <span data-ttu-id="a3fa0-251">Portál</span><span class="sxs-lookup"><span data-stu-id="a3fa0-251">Portal</span></span> | <span data-ttu-id="a3fa0-252">Gateway.azureserviceprofiler.NET</span><span class="sxs-lookup"><span data-stu-id="a3fa0-252">gateway.azureserviceprofiler.net</span></span> | <span data-ttu-id="a3fa0-253">dynamické</span><span class="sxs-lookup"><span data-stu-id="a3fa0-253">dynamic</span></span> | <span data-ttu-id="a3fa0-254">443</span><span class="sxs-lookup"><span data-stu-id="a3fa0-254">443</span></span>
| <span data-ttu-id="a3fa0-255">Úložiště</span><span class="sxs-lookup"><span data-stu-id="a3fa0-255">Storage</span></span> | <span data-ttu-id="a3fa0-256">*. core.windows.net</span><span class="sxs-lookup"><span data-stu-id="a3fa0-256">*.core.windows.net</span></span> | <span data-ttu-id="a3fa0-257">dynamické</span><span class="sxs-lookup"><span data-stu-id="a3fa0-257">dynamic</span></span> | <span data-ttu-id="a3fa0-258">443</span><span class="sxs-lookup"><span data-stu-id="a3fa0-258">443</span></span>

## <a name="snapshot-debugger"></a><span data-ttu-id="a3fa0-259">Ladicí program snímků</span><span class="sxs-lookup"><span data-stu-id="a3fa0-259">Snapshot Debugger</span></span>

| <span data-ttu-id="a3fa0-260">Účel</span><span class="sxs-lookup"><span data-stu-id="a3fa0-260">Purpose</span></span> | <span data-ttu-id="a3fa0-261">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="a3fa0-261">URI</span></span> | <span data-ttu-id="a3fa0-262">IP adresa</span><span class="sxs-lookup"><span data-stu-id="a3fa0-262">IP</span></span> | <span data-ttu-id="a3fa0-263">Porty</span><span class="sxs-lookup"><span data-stu-id="a3fa0-263">Ports</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="a3fa0-264">Agent</span><span class="sxs-lookup"><span data-stu-id="a3fa0-264">Agent</span></span> | <span data-ttu-id="a3fa0-265">ppe.azureserviceprofiler.NET</span><span class="sxs-lookup"><span data-stu-id="a3fa0-265">ppe.azureserviceprofiler.net</span></span><br/><span data-ttu-id="a3fa0-266">*. ppe.azureserviceprofiler.net</span><span class="sxs-lookup"><span data-stu-id="a3fa0-266">*.ppe.azureserviceprofiler.net</span></span> | <span data-ttu-id="a3fa0-267">dynamické</span><span class="sxs-lookup"><span data-stu-id="a3fa0-267">dynamic</span></span> | <span data-ttu-id="a3fa0-268">443</span><span class="sxs-lookup"><span data-stu-id="a3fa0-268">443</span></span>
| <span data-ttu-id="a3fa0-269">Portál</span><span class="sxs-lookup"><span data-stu-id="a3fa0-269">Portal</span></span> | <span data-ttu-id="a3fa0-270">ppe.Gateway.azureserviceprofiler.NET</span><span class="sxs-lookup"><span data-stu-id="a3fa0-270">ppe.gateway.azureserviceprofiler.net</span></span> | <span data-ttu-id="a3fa0-271">dynamické</span><span class="sxs-lookup"><span data-stu-id="a3fa0-271">dynamic</span></span> | <span data-ttu-id="a3fa0-272">443</span><span class="sxs-lookup"><span data-stu-id="a3fa0-272">443</span></span>
| <span data-ttu-id="a3fa0-273">Úložiště</span><span class="sxs-lookup"><span data-stu-id="a3fa0-273">Storage</span></span> | <span data-ttu-id="a3fa0-274">*. core.windows.net</span><span class="sxs-lookup"><span data-stu-id="a3fa0-274">*.core.windows.net</span></span> | <span data-ttu-id="a3fa0-275">dynamické</span><span class="sxs-lookup"><span data-stu-id="a3fa0-275">dynamic</span></span> | <span data-ttu-id="a3fa0-276">443</span><span class="sxs-lookup"><span data-stu-id="a3fa0-276">443</span></span>
