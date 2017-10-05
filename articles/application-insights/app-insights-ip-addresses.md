---
title: "IP adresy používané při Application Insights | Microsoft Docs"
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
ms.openlocfilehash: 3bb076c63223fc1567c6b7b25c1a513bbc81ed58
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="ip-addresses-used-by-application-insights"></a><span data-ttu-id="ed549-103">IP adresy používané službou Application Insights</span><span class="sxs-lookup"><span data-stu-id="ed549-103">IP addresses used by Application Insights</span></span>
<span data-ttu-id="ed549-104">[Azure Application Insights](app-insights-overview.md) služba používá počet IP adres.</span><span class="sxs-lookup"><span data-stu-id="ed549-104">The [Azure Application Insights](app-insights-overview.md) service uses a number of IP addresses.</span></span> <span data-ttu-id="ed549-105">Možná budete muset vědět tyto adresy, pokud je aplikace, kterou monitorujete hostované za bránou firewall.</span><span class="sxs-lookup"><span data-stu-id="ed549-105">You might need to know these addresses if the app that you are monitoring is hosted behind a firewall.</span></span>

> [!NOTE]
> <span data-ttu-id="ed549-106">I když tyto adresy jsou statické, je možné, že budeme muset změnit čas od času.</span><span class="sxs-lookup"><span data-stu-id="ed549-106">Although these addresses are static, it's possible that we will need to change them from time to time.</span></span>
> 
> 

## <a name="outgoing-ports"></a><span data-ttu-id="ed549-107">Odchozí porty</span><span class="sxs-lookup"><span data-stu-id="ed549-107">Outgoing ports</span></span>
<span data-ttu-id="ed549-108">Je třeba otevřít některé Odchozí porty v bráně firewall umožňující Application Insights SDK nebo monitorování stavu, které k odesílání dat do portálu:</span><span class="sxs-lookup"><span data-stu-id="ed549-108">You need to open some outgoing ports in your server's firewall to allow the Application Insights SDK and/or Status Monitor to send data to the portal:</span></span>

| <span data-ttu-id="ed549-109">Účel</span><span class="sxs-lookup"><span data-stu-id="ed549-109">Purpose</span></span> | <span data-ttu-id="ed549-110">ADRESA URL</span><span class="sxs-lookup"><span data-stu-id="ed549-110">URL</span></span> | <span data-ttu-id="ed549-111">IP adresa</span><span class="sxs-lookup"><span data-stu-id="ed549-111">IP</span></span> | <span data-ttu-id="ed549-112">Porty</span><span class="sxs-lookup"><span data-stu-id="ed549-112">Ports</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="ed549-113">Telemetrická data</span><span class="sxs-lookup"><span data-stu-id="ed549-113">Telemetry</span></span> |<span data-ttu-id="ed549-114">adresu DC.Services.VisualStudio.com</span><span class="sxs-lookup"><span data-stu-id="ed549-114">dc.services.visualstudio.com</span></span><br/><span data-ttu-id="ed549-115">DC.applicationinsights.microsoft.com</span><span class="sxs-lookup"><span data-stu-id="ed549-115">dc.applicationinsights.microsoft.com</span></span> |<span data-ttu-id="ed549-116">40.114.241.141</span><span class="sxs-lookup"><span data-stu-id="ed549-116">40.114.241.141</span></span><br/><span data-ttu-id="ed549-117">104.45.136.42</span><span class="sxs-lookup"><span data-stu-id="ed549-117">104.45.136.42</span></span><br/><span data-ttu-id="ed549-118">40.84.189.107</span><span class="sxs-lookup"><span data-stu-id="ed549-118">40.84.189.107</span></span><br/><span data-ttu-id="ed549-119">168.63.242.221</span><span class="sxs-lookup"><span data-stu-id="ed549-119">168.63.242.221</span></span><br/><span data-ttu-id="ed549-120">52.167.221.184</span><span class="sxs-lookup"><span data-stu-id="ed549-120">52.167.221.184</span></span><br/><span data-ttu-id="ed549-121">52.169.64.244</span><span class="sxs-lookup"><span data-stu-id="ed549-121">52.169.64.244</span></span> |<span data-ttu-id="ed549-122">443</span><span class="sxs-lookup"><span data-stu-id="ed549-122">443</span></span> |
| <span data-ttu-id="ed549-123">Živý datový proud metriky</span><span class="sxs-lookup"><span data-stu-id="ed549-123">Live Metrics Stream</span></span> |<span data-ttu-id="ed549-124">RT.Services.VisualStudio.com</span><span class="sxs-lookup"><span data-stu-id="ed549-124">rt.services.visualstudio.com</span></span><br/><span data-ttu-id="ed549-125">RT.applicationinsights.microsoft.com</span><span class="sxs-lookup"><span data-stu-id="ed549-125">rt.applicationinsights.microsoft.com</span></span> |<span data-ttu-id="ed549-126">23.96.28.38</span><span class="sxs-lookup"><span data-stu-id="ed549-126">23.96.28.38</span></span><br/><span data-ttu-id="ed549-127">13.92.40.198</span><span class="sxs-lookup"><span data-stu-id="ed549-127">13.92.40.198</span></span> |<span data-ttu-id="ed549-128">443</span><span class="sxs-lookup"><span data-stu-id="ed549-128">443</span></span> |
| <span data-ttu-id="ed549-129">Interní Telemetrie</span><span class="sxs-lookup"><span data-stu-id="ed549-129">Internal Telemetry</span></span> |<span data-ttu-id="ed549-130">breeze.aimon.applicationinsights.IO</span><span class="sxs-lookup"><span data-stu-id="ed549-130">breeze.aimon.applicationinsights.io</span></span> |<span data-ttu-id="ed549-131">52.161.11.71</span><span class="sxs-lookup"><span data-stu-id="ed549-131">52.161.11.71</span></span> |<span data-ttu-id="ed549-132">443</span><span class="sxs-lookup"><span data-stu-id="ed549-132">443</span></span> |

## <a name="status-monitor"></a><span data-ttu-id="ed549-133">Monitorování stavu</span><span class="sxs-lookup"><span data-stu-id="ed549-133">Status Monitor</span></span>
<span data-ttu-id="ed549-134">Stav monitorování konfigurace – je potřeba pouze při provádění změn.</span><span class="sxs-lookup"><span data-stu-id="ed549-134">Status Monitor Configuration - needed only when making changes.</span></span>

| <span data-ttu-id="ed549-135">Účel</span><span class="sxs-lookup"><span data-stu-id="ed549-135">Purpose</span></span> | <span data-ttu-id="ed549-136">ADRESA URL</span><span class="sxs-lookup"><span data-stu-id="ed549-136">URL</span></span> | <span data-ttu-id="ed549-137">IP adresa</span><span class="sxs-lookup"><span data-stu-id="ed549-137">IP</span></span> | <span data-ttu-id="ed549-138">Porty</span><span class="sxs-lookup"><span data-stu-id="ed549-138">Ports</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="ed549-139">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="ed549-139">Configuration</span></span> |`management.core.windows.net` | |`443` |
| <span data-ttu-id="ed549-140">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="ed549-140">Configuration</span></span> |`management.azure.com` | |`443` |
| <span data-ttu-id="ed549-141">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="ed549-141">Configuration</span></span> |`login.windows.net` | |`443` |
| <span data-ttu-id="ed549-142">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="ed549-142">Configuration</span></span> |`login.microsoftonline.com` | |`443` |
| <span data-ttu-id="ed549-143">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="ed549-143">Configuration</span></span> |`secure.aadcdn.microsoftonline-p.com` | |`443` |
| <span data-ttu-id="ed549-144">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="ed549-144">Configuration</span></span> |`auth.gfx.ms` | |`443` |
| <span data-ttu-id="ed549-145">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="ed549-145">Configuration</span></span> |`login.live.com` | |`443` |
| <span data-ttu-id="ed549-146">Instalace</span><span class="sxs-lookup"><span data-stu-id="ed549-146">Installation</span></span> |`packages.nuget.org` | |`443` |

## <a name="hockeyapp"></a><span data-ttu-id="ed549-147">HockeyApp</span><span class="sxs-lookup"><span data-stu-id="ed549-147">HockeyApp</span></span>
| <span data-ttu-id="ed549-148">Účel</span><span class="sxs-lookup"><span data-stu-id="ed549-148">Purpose</span></span> | <span data-ttu-id="ed549-149">ADRESA URL</span><span class="sxs-lookup"><span data-stu-id="ed549-149">URL</span></span> | <span data-ttu-id="ed549-150">IP adresa</span><span class="sxs-lookup"><span data-stu-id="ed549-150">IP</span></span> | <span data-ttu-id="ed549-151">Porty</span><span class="sxs-lookup"><span data-stu-id="ed549-151">Ports</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="ed549-152">Data o chybách</span><span class="sxs-lookup"><span data-stu-id="ed549-152">Crash data</span></span> |<span data-ttu-id="ed549-153">Gate.hockeyapp.NET</span><span class="sxs-lookup"><span data-stu-id="ed549-153">gate.hockeyapp.net</span></span> |<span data-ttu-id="ed549-154">104.45.136.42</span><span class="sxs-lookup"><span data-stu-id="ed549-154">104.45.136.42</span></span> |<span data-ttu-id="ed549-155">80, 443</span><span class="sxs-lookup"><span data-stu-id="ed549-155">80, 443</span></span> |

## <a name="availability-tests"></a><span data-ttu-id="ed549-156">Testy dostupnosti</span><span class="sxs-lookup"><span data-stu-id="ed549-156">Availability tests</span></span>
<span data-ttu-id="ed549-157">Toto je seznam adres, ze kterého [testy dostupnosti webu](app-insights-monitor-web-app-availability.md) spouštějí.</span><span class="sxs-lookup"><span data-stu-id="ed549-157">This is the list of addresses from which [availability web tests](app-insights-monitor-web-app-availability.md) are run.</span></span> <span data-ttu-id="ed549-158">Pokud chcete spustit testy webu ve vaší aplikaci, ale váš webový server je omezen na obsluhující konkrétní klientů, budete muset povolit příchozí provoz z našich dostupnosti testovací servery.</span><span class="sxs-lookup"><span data-stu-id="ed549-158">If you want to run web tests on your app, but your web server is restricted to serving specific clients, then you will have to permit incoming traffic from our availability test servers.</span></span>

<span data-ttu-id="ed549-159">Otevřete porty 80 (http) a 443 (https) pro příchozí provoz z těchto adres (IP adresy jsou seskupené podle umístění):</span><span class="sxs-lookup"><span data-stu-id="ed549-159">Open ports 80 (http) and 443 (https) for incoming traffic from these addresses (IP addresses are grouped by location):</span></span>

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

## <a name="data-access-api"></a><span data-ttu-id="ed549-160">Rozhraní API pro přístup k datům</span><span class="sxs-lookup"><span data-stu-id="ed549-160">Data access API</span></span>
| <span data-ttu-id="ed549-161">Účel</span><span class="sxs-lookup"><span data-stu-id="ed549-161">Purpose</span></span> | <span data-ttu-id="ed549-162">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="ed549-162">URI</span></span> | <span data-ttu-id="ed549-163">IP adresa</span><span class="sxs-lookup"><span data-stu-id="ed549-163">IP</span></span> | <span data-ttu-id="ed549-164">Porty</span><span class="sxs-lookup"><span data-stu-id="ed549-164">Ports</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="ed549-165">Rozhraní API</span><span class="sxs-lookup"><span data-stu-id="ed549-165">API</span></span> |<span data-ttu-id="ed549-166">API.applicationinsights.IO</span><span class="sxs-lookup"><span data-stu-id="ed549-166">api.applicationinsights.io</span></span><br/><span data-ttu-id="ed549-167">api1.applicationinsights.IO</span><span class="sxs-lookup"><span data-stu-id="ed549-167">api1.applicationinsights.io</span></span><br/><span data-ttu-id="ed549-168">api2.applicationinsights.IO</span><span class="sxs-lookup"><span data-stu-id="ed549-168">api2.applicationinsights.io</span></span><br/><span data-ttu-id="ed549-169">api3.applicationinsights.IO</span><span class="sxs-lookup"><span data-stu-id="ed549-169">api3.applicationinsights.io</span></span><br/><span data-ttu-id="ed549-170">api4.applicationinsights.IO</span><span class="sxs-lookup"><span data-stu-id="ed549-170">api4.applicationinsights.io</span></span><br/><span data-ttu-id="ed549-171">api5.applicationinsights.IO</span><span class="sxs-lookup"><span data-stu-id="ed549-171">api5.applicationinsights.io</span></span> |<span data-ttu-id="ed549-172">13.82.26.252</span><span class="sxs-lookup"><span data-stu-id="ed549-172">13.82.26.252</span></span><br/><span data-ttu-id="ed549-173">40.76.213.73</span><span class="sxs-lookup"><span data-stu-id="ed549-173">40.76.213.73</span></span> |<span data-ttu-id="ed549-174">80,443</span><span class="sxs-lookup"><span data-stu-id="ed549-174">80,443</span></span> |
| <span data-ttu-id="ed549-175">Dokumentace rozhraní API</span><span class="sxs-lookup"><span data-stu-id="ed549-175">API docs</span></span> |<span data-ttu-id="ed549-176">dev.applicationinsights.IO</span><span class="sxs-lookup"><span data-stu-id="ed549-176">dev.applicationinsights.io</span></span><br/><span data-ttu-id="ed549-177">dev.applicationinsights.microsoft.com</span><span class="sxs-lookup"><span data-stu-id="ed549-177">dev.applicationinsights.microsoft.com</span></span><br/><span data-ttu-id="ed549-178">dev.aisvc.VisualStudio.com</span><span class="sxs-lookup"><span data-stu-id="ed549-178">dev.aisvc.visualstudio.com</span></span><br/><span data-ttu-id="ed549-179">www.applicationinsights.IO</span><span class="sxs-lookup"><span data-stu-id="ed549-179">www.applicationinsights.io</span></span><br/><span data-ttu-id="ed549-180">www.applicationinsights.microsoft.com</span><span class="sxs-lookup"><span data-stu-id="ed549-180">www.applicationinsights.microsoft.com</span></span><br/><span data-ttu-id="ed549-181">www.aisvc.VisualStudio.com</span><span class="sxs-lookup"><span data-stu-id="ed549-181">www.aisvc.visualstudio.com</span></span> |<span data-ttu-id="ed549-182">13.82.24.149</span><span class="sxs-lookup"><span data-stu-id="ed549-182">13.82.24.149</span></span><br/><span data-ttu-id="ed549-183">40.114.82.10</span><span class="sxs-lookup"><span data-stu-id="ed549-183">40.114.82.10</span></span> |<span data-ttu-id="ed549-184">80,443</span><span class="sxs-lookup"><span data-stu-id="ed549-184">80,443</span></span> |
| <span data-ttu-id="ed549-185">Vnitřní rozhraní API</span><span class="sxs-lookup"><span data-stu-id="ed549-185">Internal API</span></span> |<span data-ttu-id="ed549-186">aigs.aisvc.VisualStudio.com</span><span class="sxs-lookup"><span data-stu-id="ed549-186">aigs.aisvc.visualstudio.com</span></span><br/><span data-ttu-id="ed549-187">aigs1.aisvc.VisualStudio.com</span><span class="sxs-lookup"><span data-stu-id="ed549-187">aigs1.aisvc.visualstudio.com</span></span><br/><span data-ttu-id="ed549-188">aigs2.aisvc.VisualStudio.com</span><span class="sxs-lookup"><span data-stu-id="ed549-188">aigs2.aisvc.visualstudio.com</span></span><br/><span data-ttu-id="ed549-189">aigs3.aisvc.VisualStudio.com</span><span class="sxs-lookup"><span data-stu-id="ed549-189">aigs3.aisvc.visualstudio.com</span></span><br/><span data-ttu-id="ed549-190">aigs4.aisvc.VisualStudio.com</span><span class="sxs-lookup"><span data-stu-id="ed549-190">aigs4.aisvc.visualstudio.com</span></span><br/><span data-ttu-id="ed549-191">aigs5.aisvc.VisualStudio.com</span><span class="sxs-lookup"><span data-stu-id="ed549-191">aigs5.aisvc.visualstudio.com</span></span><br/><span data-ttu-id="ed549-192">aigs6.aisvc.VisualStudio.com</span><span class="sxs-lookup"><span data-stu-id="ed549-192">aigs6.aisvc.visualstudio.com</span></span> |<span data-ttu-id="ed549-193">dynamické</span><span class="sxs-lookup"><span data-stu-id="ed549-193">dynamic</span></span>|<span data-ttu-id="ed549-194">443</span><span class="sxs-lookup"><span data-stu-id="ed549-194">443</span></span> |

## <a name="application-insights-analytics"></a><span data-ttu-id="ed549-195">Analýza statistiky aplikace</span><span class="sxs-lookup"><span data-stu-id="ed549-195">Application Insights Analytics</span></span>

| <span data-ttu-id="ed549-196">Účel</span><span class="sxs-lookup"><span data-stu-id="ed549-196">Purpose</span></span> | <span data-ttu-id="ed549-197">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="ed549-197">URI</span></span> | <span data-ttu-id="ed549-198">IP adresa</span><span class="sxs-lookup"><span data-stu-id="ed549-198">IP</span></span> | <span data-ttu-id="ed549-199">Porty</span><span class="sxs-lookup"><span data-stu-id="ed549-199">Ports</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="ed549-200">Portálu analýza</span><span class="sxs-lookup"><span data-stu-id="ed549-200">Analytics Portal</span></span> | <span data-ttu-id="ed549-201">Analytics.applicationinsights.IO</span><span class="sxs-lookup"><span data-stu-id="ed549-201">analytics.applicationinsights.io</span></span> | <span data-ttu-id="ed549-202">dynamické</span><span class="sxs-lookup"><span data-stu-id="ed549-202">dynamic</span></span> | <span data-ttu-id="ed549-203">80,443</span><span class="sxs-lookup"><span data-stu-id="ed549-203">80,443</span></span> |
| <span data-ttu-id="ed549-204">CDN</span><span class="sxs-lookup"><span data-stu-id="ed549-204">CDN</span></span> | <span data-ttu-id="ed549-205">applicationanalytics.azureedge.NET</span><span class="sxs-lookup"><span data-stu-id="ed549-205">applicationanalytics.azureedge.net</span></span> | <span data-ttu-id="ed549-206">dynamické</span><span class="sxs-lookup"><span data-stu-id="ed549-206">dynamic</span></span> | <span data-ttu-id="ed549-207">80,443</span><span class="sxs-lookup"><span data-stu-id="ed549-207">80,443</span></span> |
| <span data-ttu-id="ed549-208">Média CDN</span><span class="sxs-lookup"><span data-stu-id="ed549-208">Media CDN</span></span> | <span data-ttu-id="ed549-209">applicationanalyticsmedia.azureedge.NET</span><span class="sxs-lookup"><span data-stu-id="ed549-209">applicationanalyticsmedia.azureedge.net</span></span> | <span data-ttu-id="ed549-210">dynamické</span><span class="sxs-lookup"><span data-stu-id="ed549-210">dynamic</span></span> | <span data-ttu-id="ed549-211">80,443</span><span class="sxs-lookup"><span data-stu-id="ed549-211">80,443</span></span> |

<span data-ttu-id="ed549-212">Poznámka: *. vlastní domény applicationinsights.io týmem Application Insights.</span><span class="sxs-lookup"><span data-stu-id="ed549-212">Note: *.applicationinsights.io domain is owned by Application Insights team.</span></span>

## <a name="application-insights-azure-portal-extension"></a><span data-ttu-id="ed549-213">Application Insights rozšíření portálu Azure</span><span class="sxs-lookup"><span data-stu-id="ed549-213">Application Insights Azure Portal Extension</span></span>

| <span data-ttu-id="ed549-214">Účel</span><span class="sxs-lookup"><span data-stu-id="ed549-214">Purpose</span></span> | <span data-ttu-id="ed549-215">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="ed549-215">URI</span></span> | <span data-ttu-id="ed549-216">IP adresa</span><span class="sxs-lookup"><span data-stu-id="ed549-216">IP</span></span> | <span data-ttu-id="ed549-217">Porty</span><span class="sxs-lookup"><span data-stu-id="ed549-217">Ports</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="ed549-218">Rozšíření Application Insights</span><span class="sxs-lookup"><span data-stu-id="ed549-218">Application Insights Extension</span></span> | <span data-ttu-id="ed549-219">stamp2.app.insightsportal.VisualStudio.com</span><span class="sxs-lookup"><span data-stu-id="ed549-219">stamp2.app.insightsportal.visualstudio.com</span></span> | <span data-ttu-id="ed549-220">dynamické</span><span class="sxs-lookup"><span data-stu-id="ed549-220">dynamic</span></span> | <span data-ttu-id="ed549-221">80,443</span><span class="sxs-lookup"><span data-stu-id="ed549-221">80,443</span></span> |
| <span data-ttu-id="ed549-222">Application Insights rozšíření CDN</span><span class="sxs-lookup"><span data-stu-id="ed549-222">Application Insights Extension CDN</span></span> | <span data-ttu-id="ed549-223">insightsportal. prod2 cdn.aisvc.visualstudio.com</span><span class="sxs-lookup"><span data-stu-id="ed549-223">insightsportal-prod2-cdn.aisvc.visualstudio.com</span></span><br/><span data-ttu-id="ed549-224">insightsportal prod2 asiae cdn.aisvc.visualstudio.com</span><span class="sxs-lookup"><span data-stu-id="ed549-224">insightsportal-prod2-asiae-cdn.aisvc.visualstudio.com</span></span><br/><span data-ttu-id="ed549-225">insightsportal-cdn-aimon.applicationinsights.io</span><span class="sxs-lookup"><span data-stu-id="ed549-225">insightsportal-cdn-aimon.applicationinsights.io</span></span> | <span data-ttu-id="ed549-226">dynamické</span><span class="sxs-lookup"><span data-stu-id="ed549-226">dynamic</span></span> | <span data-ttu-id="ed549-227">80,443</span><span class="sxs-lookup"><span data-stu-id="ed549-227">80,443</span></span> |

## <a name="application-insights-sdks"></a><span data-ttu-id="ed549-228">Sadách Application Insights SDK</span><span class="sxs-lookup"><span data-stu-id="ed549-228">Application Insights SDKs</span></span>

| <span data-ttu-id="ed549-229">Účel</span><span class="sxs-lookup"><span data-stu-id="ed549-229">Purpose</span></span> | <span data-ttu-id="ed549-230">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="ed549-230">URI</span></span> | <span data-ttu-id="ed549-231">IP adresa</span><span class="sxs-lookup"><span data-stu-id="ed549-231">IP</span></span> | <span data-ttu-id="ed549-232">Porty</span><span class="sxs-lookup"><span data-stu-id="ed549-232">Ports</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="ed549-233">Application Insights JS SDK CDN</span><span class="sxs-lookup"><span data-stu-id="ed549-233">Application Insights JS SDK CDN</span></span> | <span data-ttu-id="ed549-234">az416426.vo.msecnd.NET</span><span class="sxs-lookup"><span data-stu-id="ed549-234">az416426.vo.msecnd.net</span></span> | <span data-ttu-id="ed549-235">dynamické</span><span class="sxs-lookup"><span data-stu-id="ed549-235">dynamic</span></span> | <span data-ttu-id="ed549-236">80,443</span><span class="sxs-lookup"><span data-stu-id="ed549-236">80,443</span></span> |
| <span data-ttu-id="ed549-237">Application Insights Java SDK</span><span class="sxs-lookup"><span data-stu-id="ed549-237">Application Insights Java SDK</span></span> | <span data-ttu-id="ed549-238">aijavasdk.BLOB.Core.Windows.NET</span><span class="sxs-lookup"><span data-stu-id="ed549-238">aijavasdk.blob.core.windows.net</span></span> | <span data-ttu-id="ed549-239">dynamické</span><span class="sxs-lookup"><span data-stu-id="ed549-239">dynamic</span></span> | <span data-ttu-id="ed549-240">80,443</span><span class="sxs-lookup"><span data-stu-id="ed549-240">80,443</span></span> |

## <a name="profiler"></a><span data-ttu-id="ed549-241">Profiler</span><span class="sxs-lookup"><span data-stu-id="ed549-241">Profiler</span></span>

| <span data-ttu-id="ed549-242">Účel</span><span class="sxs-lookup"><span data-stu-id="ed549-242">Purpose</span></span> | <span data-ttu-id="ed549-243">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="ed549-243">URI</span></span> | <span data-ttu-id="ed549-244">IP adresa</span><span class="sxs-lookup"><span data-stu-id="ed549-244">IP</span></span> | <span data-ttu-id="ed549-245">Porty</span><span class="sxs-lookup"><span data-stu-id="ed549-245">Ports</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="ed549-246">Agent</span><span class="sxs-lookup"><span data-stu-id="ed549-246">Agent</span></span> | <span data-ttu-id="ed549-247">Agent.azureserviceprofiler.NET</span><span class="sxs-lookup"><span data-stu-id="ed549-247">agent.azureserviceprofiler.net</span></span><br/><span data-ttu-id="ed549-248">*. agent.azureserviceprofiler.net</span><span class="sxs-lookup"><span data-stu-id="ed549-248">*.agent.azureserviceprofiler.net</span></span> | <span data-ttu-id="ed549-249">dynamické</span><span class="sxs-lookup"><span data-stu-id="ed549-249">dynamic</span></span> | <span data-ttu-id="ed549-250">443</span><span class="sxs-lookup"><span data-stu-id="ed549-250">443</span></span>
| <span data-ttu-id="ed549-251">Portál</span><span class="sxs-lookup"><span data-stu-id="ed549-251">Portal</span></span> | <span data-ttu-id="ed549-252">Gateway.azureserviceprofiler.NET</span><span class="sxs-lookup"><span data-stu-id="ed549-252">gateway.azureserviceprofiler.net</span></span> | <span data-ttu-id="ed549-253">dynamické</span><span class="sxs-lookup"><span data-stu-id="ed549-253">dynamic</span></span> | <span data-ttu-id="ed549-254">443</span><span class="sxs-lookup"><span data-stu-id="ed549-254">443</span></span>
| <span data-ttu-id="ed549-255">Úložiště</span><span class="sxs-lookup"><span data-stu-id="ed549-255">Storage</span></span> | <span data-ttu-id="ed549-256">*. core.windows.net</span><span class="sxs-lookup"><span data-stu-id="ed549-256">*.core.windows.net</span></span> | <span data-ttu-id="ed549-257">dynamické</span><span class="sxs-lookup"><span data-stu-id="ed549-257">dynamic</span></span> | <span data-ttu-id="ed549-258">443</span><span class="sxs-lookup"><span data-stu-id="ed549-258">443</span></span>

## <a name="snapshot-debugger"></a><span data-ttu-id="ed549-259">Ladicí program snímků</span><span class="sxs-lookup"><span data-stu-id="ed549-259">Snapshot Debugger</span></span>

| <span data-ttu-id="ed549-260">Účel</span><span class="sxs-lookup"><span data-stu-id="ed549-260">Purpose</span></span> | <span data-ttu-id="ed549-261">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="ed549-261">URI</span></span> | <span data-ttu-id="ed549-262">IP adresa</span><span class="sxs-lookup"><span data-stu-id="ed549-262">IP</span></span> | <span data-ttu-id="ed549-263">Porty</span><span class="sxs-lookup"><span data-stu-id="ed549-263">Ports</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="ed549-264">Agent</span><span class="sxs-lookup"><span data-stu-id="ed549-264">Agent</span></span> | <span data-ttu-id="ed549-265">ppe.azureserviceprofiler.NET</span><span class="sxs-lookup"><span data-stu-id="ed549-265">ppe.azureserviceprofiler.net</span></span><br/><span data-ttu-id="ed549-266">*. ppe.azureserviceprofiler.net</span><span class="sxs-lookup"><span data-stu-id="ed549-266">*.ppe.azureserviceprofiler.net</span></span> | <span data-ttu-id="ed549-267">dynamické</span><span class="sxs-lookup"><span data-stu-id="ed549-267">dynamic</span></span> | <span data-ttu-id="ed549-268">443</span><span class="sxs-lookup"><span data-stu-id="ed549-268">443</span></span>
| <span data-ttu-id="ed549-269">Portál</span><span class="sxs-lookup"><span data-stu-id="ed549-269">Portal</span></span> | <span data-ttu-id="ed549-270">ppe.Gateway.azureserviceprofiler.NET</span><span class="sxs-lookup"><span data-stu-id="ed549-270">ppe.gateway.azureserviceprofiler.net</span></span> | <span data-ttu-id="ed549-271">dynamické</span><span class="sxs-lookup"><span data-stu-id="ed549-271">dynamic</span></span> | <span data-ttu-id="ed549-272">443</span><span class="sxs-lookup"><span data-stu-id="ed549-272">443</span></span>
| <span data-ttu-id="ed549-273">Úložiště</span><span class="sxs-lookup"><span data-stu-id="ed549-273">Storage</span></span> | <span data-ttu-id="ed549-274">*. core.windows.net</span><span class="sxs-lookup"><span data-stu-id="ed549-274">*.core.windows.net</span></span> | <span data-ttu-id="ed549-275">dynamické</span><span class="sxs-lookup"><span data-stu-id="ed549-275">dynamic</span></span> | <span data-ttu-id="ed549-276">443</span><span class="sxs-lookup"><span data-stu-id="ed549-276">443</span></span>
