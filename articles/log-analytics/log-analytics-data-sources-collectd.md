---
title: aaaCollect data z CollectD v OMS Log Analytics | Microsoft Docs
description: "CollectD je démon Linux s otevřeným zdrojem, který pravidelně shromažďuje data z aplikace a informace o úrovni systému.  Tento článek obsahuje informace o shromažďování dat z CollectD v analýzy protokolů."
services: log-analytics
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: f1d5bde4-6b86-4b8e-b5c1-3ecbaba76198
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/02/2017
ms.author: magoedte
ms.openlocfilehash: 7ad82c9c67a664aabd44f08bef2253d84cd2dfba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="collect-data-from-collectd-on-linux-agents-in-log-analytics"></a><span data-ttu-id="d7931-104">Shromažďování dat z CollectD na agentech Linux v analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="d7931-104">Collect data from CollectD on Linux agents in Log Analytics</span></span>
<span data-ttu-id="d7931-105">[CollectD](https://collectd.org/) je démon Linux s otevřeným zdrojem, který pravidelně shromažďuje metriky výkonu z aplikace a informace o úrovni systému.</span><span class="sxs-lookup"><span data-stu-id="d7931-105">[CollectD](https://collectd.org/) is an open source Linux daemon that periodically collects performance metrics from applications and system level information.</span></span> <span data-ttu-id="d7931-106">Příklad aplikace zahrnovat hello Java Virtual Machine (JVM), MySQL Server a Nginx.</span><span class="sxs-lookup"><span data-stu-id="d7931-106">Example applications include hello Java Virtual Machine (JVM), MySQL Server, and Nginx.</span></span> <span data-ttu-id="d7931-107">Tento článek obsahuje informace o shromažďování dat výkonu z CollectD v analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="d7931-107">This article provides information on collecting performance data from CollectD in Log Analytics.</span></span>

<span data-ttu-id="d7931-108">Úplný seznam dostupných modulů plug-in najdete na [tabulky modulů plug-in](https://collectd.org/wiki/index.php/Table_of_Plugins).</span><span class="sxs-lookup"><span data-stu-id="d7931-108">A full list of available plugins can be found at [Table of Plugins](https://collectd.org/wiki/index.php/Table_of_Plugins).</span></span>

![Přehled CollectD](media/log-analytics-data-sources-collectd/overview.png)

<span data-ttu-id="d7931-110">Hello následující konfiguraci CollectD je součástí hello OMS agenta pro Linux tooroute CollectD data toohello OMS agenta pro Linux.</span><span class="sxs-lookup"><span data-stu-id="d7931-110">hello following CollectD configuration is included in hello OMS Agent for Linux tooroute  CollectD data toohello OMS Agent for Linux.</span></span>

    LoadPlugin write_http

    <Plugin write_http>
         <Node "oms">
         URL "127.0.0.1:26000/oms.collectd"
         Format "JSON"
         StoreRates true
         </Node>
    </Plugin>

<span data-ttu-id="d7931-111">Kromě toho pokud se používá verzích collectD než 5.5 použít hello místo následující konfigurace.</span><span class="sxs-lookup"><span data-stu-id="d7931-111">Additionally, if using an versions of collectD before 5.5 use hello following configuration instead.</span></span>

    LoadPlugin write_http

    <Plugin write_http>
       <URL "127.0.0.1:26000/oms.collectd">
        Format "JSON"
         StoreRates true
       </URL>
    </Plugin>

<span data-ttu-id="d7931-112">Konfigurace CollectD Hello používá výchozí hello`write_http` metriky dat výkonu modulu plug-in toosend přes port 26000 tooOMS agenta pro Linux.</span><span class="sxs-lookup"><span data-stu-id="d7931-112">hello CollectD configuration uses hello default`write_http` plugin toosend performance metric data over port 26000 tooOMS Agent for Linux.</span></span> 

> [!NOTE]
> <span data-ttu-id="d7931-113">Tento port může být nakonfigurované tooa vlastní port, v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="d7931-113">This port can be configured tooa custom-defined port if needed.</span></span>

<span data-ttu-id="d7931-114">Hello OMS agenta pro Linux také naslouchá na portu 26000 CollectD metrik a převede je tooOMS schématu metriky.</span><span class="sxs-lookup"><span data-stu-id="d7931-114">hello OMS Agent for Linux also listens on port 26000 for CollectD metrics and then converts them tooOMS schema metrics.</span></span> <span data-ttu-id="d7931-115">Hello následuje hello OMS agenta pro Linux konfiguraci `collectd.conf`.</span><span class="sxs-lookup"><span data-stu-id="d7931-115">hello following is hello OMS Agent for Linux configuration  `collectd.conf`.</span></span>

    <source>
      type http
      port 26000
      bind 127.0.0.1
    </source>

    <filter oms.collectd>
      type filter_collectd
    </filter>


## <a name="versions-supported"></a><span data-ttu-id="d7931-116">Podporované verze</span><span class="sxs-lookup"><span data-stu-id="d7931-116">Versions supported</span></span>
- <span data-ttu-id="d7931-117">Aktuálně podporuje CollectD verze 4.8 analýzy protokolů a vyšší.</span><span class="sxs-lookup"><span data-stu-id="d7931-117">Log Analytics currently supports CollectD version 4.8 and above.</span></span>
- <span data-ttu-id="d7931-118">Je vyžadována pro kolekci metriky CollectD OMS agenta pro Linux v1.1.0-217 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="d7931-118">OMS Agent for Linux v1.1.0-217 or above is required for CollectD metric collection.</span></span>


## <a name="configuration"></a><span data-ttu-id="d7931-119">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="d7931-119">Configuration</span></span>
<span data-ttu-id="d7931-120">Hello následují základní kroky tooconfigure kolekce dat CollectD v analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="d7931-120">hello following are basic steps tooconfigure collection of CollectD data in Log Analytics.</span></span>

1. <span data-ttu-id="d7931-121">Nakonfigurujte CollectD toosend data toohello OMS agenta pro Linux pomocí modulu plug-in write_http hello.</span><span class="sxs-lookup"><span data-stu-id="d7931-121">Configure CollectD toosend data toohello OMS Agent for Linux using hello write_http plugin.</span></span>  
2. <span data-ttu-id="d7931-122">Nakonfigurujte hello OMS agenta pro Linux toolisten pro hello CollectD data na příslušný port hello.</span><span class="sxs-lookup"><span data-stu-id="d7931-122">Configure hello OMS Agent for Linux toolisten for hello CollectD data on hello appropriate port.</span></span>
3. <span data-ttu-id="d7931-123">Restartujte CollectD a OMS agenta pro Linux.</span><span class="sxs-lookup"><span data-stu-id="d7931-123">Restart CollectD and OMS Agent for Linux.</span></span>

### <a name="configure-collectd-tooforward-data"></a><span data-ttu-id="d7931-124">Konfigurace CollectD tooforward dat</span><span class="sxs-lookup"><span data-stu-id="d7931-124">Configure CollectD tooforward data</span></span> 

1. <span data-ttu-id="d7931-125">tooroute CollectD data toohello OMS agenta pro Linux, `oms.conf` toobe musí přidat na tooCollectD konfiguračního adresáře.</span><span class="sxs-lookup"><span data-stu-id="d7931-125">tooroute CollectD data toohello OMS Agent for Linux, `oms.conf` needs toobe added tooCollectD's configuration directory.</span></span> <span data-ttu-id="d7931-126">Cílový Hello tohoto souboru závisí na hello Linux distro počítače.</span><span class="sxs-lookup"><span data-stu-id="d7931-126">hello destination of this file depends on hello Linux  distro of your machine.</span></span>

    <span data-ttu-id="d7931-127">Pokud konfigurace adresáře CollectD nachází ve /etc/collectd.d/:</span><span class="sxs-lookup"><span data-stu-id="d7931-127">If your CollectD config directory is located in /etc/collectd.d/:</span></span>

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/oms.conf /etc/collectd.d/oms.conf

    <span data-ttu-id="d7931-128">Pokud konfigurace adresáře CollectD nachází ve /etc/collectd/collectd.conf.d/:</span><span class="sxs-lookup"><span data-stu-id="d7931-128">If your CollectD config directory is located in /etc/collectd/collectd.conf.d/:</span></span>

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/oms.conf /etc/collectd/collectd.conf.d/oms.conf

    >[!NOTE]
    ><span data-ttu-id="d7931-129">Verze CollectD před 5.5 bude mít toomodify hello značky `oms.conf` jako v příkladu nahoře.</span><span class="sxs-lookup"><span data-stu-id="d7931-129">For CollectD versions before 5.5 you will have toomodify hello tags in `oms.conf` as shown above.</span></span>
    >

2. <span data-ttu-id="d7931-130">Zkopírujte prostoru toohello potřeby collectd.conf omsagent konfiguračního adresáře.</span><span class="sxs-lookup"><span data-stu-id="d7931-130">Copy collectd.conf toohello desired workspace's omsagent configuration directory.</span></span>

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/collectd.conf /etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/
        sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/collectd.conf

3. <span data-ttu-id="d7931-131">Restartujte CollectD a OMS agenta pro Linux s hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="d7931-131">Restart CollectD and OMS Agent for Linux with hello following commands.</span></span>

    <span data-ttu-id="d7931-132">sudo služby collectd restartování sudo /opt/microsoft/omsagent/bin/service_control restartování</span><span class="sxs-lookup"><span data-stu-id="d7931-132">sudo service collectd restart  sudo /opt/microsoft/omsagent/bin/service_control restart</span></span>

## <a name="collectd-metrics-toolog-analytics-schema-conversion"></a><span data-ttu-id="d7931-133">CollectD metriky tooLog Analytics schématu převod</span><span class="sxs-lookup"><span data-stu-id="d7931-133">CollectD metrics tooLog Analytics schema conversion</span></span>
<span data-ttu-id="d7931-134">toomaintain známý model mezi infrastruktura metriky již shromážděné agentem OMS pro systémy Linux a hello nové metriky shromážděné CollectD hello následující schéma mapování se používá:</span><span class="sxs-lookup"><span data-stu-id="d7931-134">toomaintain a familiar model between infrastructure metrics already collected by OMS Agent for Linux and hello new metrics collected by CollectD hello following schema mapping is used:</span></span>

| <span data-ttu-id="d7931-135">Metrika CollectD pole</span><span class="sxs-lookup"><span data-stu-id="d7931-135">CollectD Metric field</span></span> | <span data-ttu-id="d7931-136">Pole analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="d7931-136">Log Analytics field</span></span> |
|:--|:--|
| <span data-ttu-id="d7931-137">hostitele</span><span class="sxs-lookup"><span data-stu-id="d7931-137">host</span></span> | <span data-ttu-id="d7931-138">Počítač</span><span class="sxs-lookup"><span data-stu-id="d7931-138">Computer</span></span> |
| <span data-ttu-id="d7931-139">Modul plug-in</span><span class="sxs-lookup"><span data-stu-id="d7931-139">plugin</span></span> | <span data-ttu-id="d7931-140">Žádný</span><span class="sxs-lookup"><span data-stu-id="d7931-140">None</span></span> |
| <span data-ttu-id="d7931-141">plugin_instance</span><span class="sxs-lookup"><span data-stu-id="d7931-141">plugin_instance</span></span> | <span data-ttu-id="d7931-142">Název instance</span><span class="sxs-lookup"><span data-stu-id="d7931-142">Instance Name</span></span><br><span data-ttu-id="d7931-143">Pokud **plugin_instance** je *null* pak InstanceName = "*_celkem*"</span><span class="sxs-lookup"><span data-stu-id="d7931-143">If **plugin_instance** is *null* then InstanceName="*_Total*"</span></span> |
| <span data-ttu-id="d7931-144">type</span><span class="sxs-lookup"><span data-stu-id="d7931-144">type</span></span> | <span data-ttu-id="d7931-145">Název objektu</span><span class="sxs-lookup"><span data-stu-id="d7931-145">ObjectName</span></span> |
| <span data-ttu-id="d7931-146">type_instance</span><span class="sxs-lookup"><span data-stu-id="d7931-146">type_instance</span></span> | <span data-ttu-id="d7931-147">Název_čítače</span><span class="sxs-lookup"><span data-stu-id="d7931-147">CounterName</span></span><br><span data-ttu-id="d7931-148">Pokud **type_instance** je *null* pak CounterName =**prázdné**</span><span class="sxs-lookup"><span data-stu-id="d7931-148">If **type_instance** is *null* then CounterName=**blank**</span></span> |
| <span data-ttu-id="d7931-149">[dsnames]</span><span class="sxs-lookup"><span data-stu-id="d7931-149">dsnames[]</span></span> | <span data-ttu-id="d7931-150">Název_čítače</span><span class="sxs-lookup"><span data-stu-id="d7931-150">CounterName</span></span> |
| <span data-ttu-id="d7931-151">dstypes</span><span class="sxs-lookup"><span data-stu-id="d7931-151">dstypes</span></span> | <span data-ttu-id="d7931-152">Žádný</span><span class="sxs-lookup"><span data-stu-id="d7931-152">None</span></span> |
| <span data-ttu-id="d7931-153">[] – hodnoty</span><span class="sxs-lookup"><span data-stu-id="d7931-153">values[]</span></span> | <span data-ttu-id="d7931-154">Přepočtené</span><span class="sxs-lookup"><span data-stu-id="d7931-154">CounterValue</span></span> |

## <a name="next-steps"></a><span data-ttu-id="d7931-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d7931-155">Next steps</span></span>
* <span data-ttu-id="d7931-156">Další informace o [protokolu hledání](log-analytics-log-searches.md) tooanalyze hello data budou shromažďovat ze zdroje dat a řešení.</span><span class="sxs-lookup"><span data-stu-id="d7931-156">Learn about [log searches](log-analytics-log-searches.md) tooanalyze hello data collected from data sources and solutions.</span></span> 
* <span data-ttu-id="d7931-157">Použití [vlastní pole](log-analytics-custom-fields.md) tooparse data z syslog záznamů do jednotlivých polí.</span><span class="sxs-lookup"><span data-stu-id="d7931-157">Use [Custom Fields](log-analytics-custom-fields.md) tooparse data from syslog records into individual fields.</span></span>

