---
title: "Shromažďování dat z CollectD v OMS Log Analytics | Microsoft Docs"
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
ms.openlocfilehash: a63b15ca5126b45451f0694c9ee75d7b67b1ceaf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="collect-data-from-collectd-on-linux-agents-in-log-analytics"></a><span data-ttu-id="09075-104">Shromažďování dat z CollectD na agentech Linux v analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="09075-104">Collect data from CollectD on Linux agents in Log Analytics</span></span>
<span data-ttu-id="09075-105">[CollectD](https://collectd.org/) je démon Linux s otevřeným zdrojem, který pravidelně shromažďuje metriky výkonu z aplikace a informace o úrovni systému.</span><span class="sxs-lookup"><span data-stu-id="09075-105">[CollectD](https://collectd.org/) is an open source Linux daemon that periodically collects performance metrics from applications and system level information.</span></span> <span data-ttu-id="09075-106">Příklad aplikace patří Java Virtual Machine (JVM), MySQL Server a Nginx.</span><span class="sxs-lookup"><span data-stu-id="09075-106">Example applications include the Java Virtual Machine (JVM), MySQL Server, and Nginx.</span></span> <span data-ttu-id="09075-107">Tento článek obsahuje informace o shromažďování dat výkonu z CollectD v analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="09075-107">This article provides information on collecting performance data from CollectD in Log Analytics.</span></span>

<span data-ttu-id="09075-108">Úplný seznam dostupných modulů plug-in najdete na [tabulky modulů plug-in](https://collectd.org/wiki/index.php/Table_of_Plugins).</span><span class="sxs-lookup"><span data-stu-id="09075-108">A full list of available plugins can be found at [Table of Plugins](https://collectd.org/wiki/index.php/Table_of_Plugins).</span></span>

![Přehled CollectD](media/log-analytics-data-sources-collectd/overview.png)

<span data-ttu-id="09075-110">Následující konfigurace CollectD je součástí agenta OMS pro Linux tak, aby data trasy CollectD OMS agenta pro Linux.</span><span class="sxs-lookup"><span data-stu-id="09075-110">The following CollectD configuration is included in the OMS Agent for Linux to route  CollectD data to the OMS Agent for Linux.</span></span>

    LoadPlugin write_http

    <Plugin write_http>
         <Node "oms">
         URL "127.0.0.1:26000/oms.collectd"
         Format "JSON"
         StoreRates true
         </Node>
    </Plugin>

<span data-ttu-id="09075-111">Kromě toho pokud se používá verzích collectD před 5.5 místo toho použijte následující konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="09075-111">Additionally, if using an versions of collectD before 5.5 use the following configuration instead.</span></span>

    LoadPlugin write_http

    <Plugin write_http>
       <URL "127.0.0.1:26000/oms.collectd">
        Format "JSON"
         StoreRates true
       </URL>
    </Plugin>

<span data-ttu-id="09075-112">Konfigurace CollectD používá výchozí`write_http` modulu plug-in k odesílání dat metriky výkonu přes port 26000 do OMS agenta pro Linux.</span><span class="sxs-lookup"><span data-stu-id="09075-112">The CollectD configuration uses the default`write_http` plugin to send performance metric data over port 26000 to OMS Agent for Linux.</span></span> 

> [!NOTE]
> <span data-ttu-id="09075-113">Tento port lze nakonfigurovat na vlastní port, v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="09075-113">This port can be configured to a custom-defined port if needed.</span></span>

<span data-ttu-id="09075-114">OMS agenta pro Linux také naslouchá na portu 26000 CollectD metrik a převede je na OMS schématu metriky.</span><span class="sxs-lookup"><span data-stu-id="09075-114">The OMS Agent for Linux also listens on port 26000 for CollectD metrics and then converts them to OMS schema metrics.</span></span> <span data-ttu-id="09075-115">Tady je OMS agenta pro Linux konfiguraci `collectd.conf`.</span><span class="sxs-lookup"><span data-stu-id="09075-115">The following is the OMS Agent for Linux configuration  `collectd.conf`.</span></span>

    <source>
      type http
      port 26000
      bind 127.0.0.1
    </source>

    <filter oms.collectd>
      type filter_collectd
    </filter>


## <a name="versions-supported"></a><span data-ttu-id="09075-116">Podporované verze</span><span class="sxs-lookup"><span data-stu-id="09075-116">Versions supported</span></span>
- <span data-ttu-id="09075-117">Aktuálně podporuje CollectD verze 4.8 analýzy protokolů a vyšší.</span><span class="sxs-lookup"><span data-stu-id="09075-117">Log Analytics currently supports CollectD version 4.8 and above.</span></span>
- <span data-ttu-id="09075-118">Je vyžadována pro kolekci metriky CollectD OMS agenta pro Linux v1.1.0-217 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="09075-118">OMS Agent for Linux v1.1.0-217 or above is required for CollectD metric collection.</span></span>


## <a name="configuration"></a><span data-ttu-id="09075-119">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="09075-119">Configuration</span></span>
<span data-ttu-id="09075-120">Následují základní kroky konfigurace sběru dat CollectD v analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="09075-120">The following are basic steps to configure collection of CollectD data in Log Analytics.</span></span>

1. <span data-ttu-id="09075-121">Nakonfigurujte CollectD k odesílání dat do OMS agenta pro Linux pomocí modulu plug-in write_http.</span><span class="sxs-lookup"><span data-stu-id="09075-121">Configure CollectD to send data to the OMS Agent for Linux using the write_http plugin.</span></span>  
2. <span data-ttu-id="09075-122">Konfigurace agenta OMS pro Linux naslouchat CollectD data na příslušný port.</span><span class="sxs-lookup"><span data-stu-id="09075-122">Configure the OMS Agent for Linux to listen for the CollectD data on the appropriate port.</span></span>
3. <span data-ttu-id="09075-123">Restartujte CollectD a OMS agenta pro Linux.</span><span class="sxs-lookup"><span data-stu-id="09075-123">Restart CollectD and OMS Agent for Linux.</span></span>

### <a name="configure-collectd-to-forward-data"></a><span data-ttu-id="09075-124">Konfigurace CollectD k předávání dat</span><span class="sxs-lookup"><span data-stu-id="09075-124">Configure CollectD to forward data</span></span> 

1. <span data-ttu-id="09075-125">K datům CollectD trasy a OMS agenta pro Linux `oms.conf` musí být přidán do adresáře společnosti CollectD konfigurace.</span><span class="sxs-lookup"><span data-stu-id="09075-125">To route CollectD data to the OMS Agent for Linux, `oms.conf` needs to be added to CollectD's configuration directory.</span></span> <span data-ttu-id="09075-126">Cílem tohoto souboru závisí na distro Linux počítače.</span><span class="sxs-lookup"><span data-stu-id="09075-126">The destination of this file depends on the Linux  distro of your machine.</span></span>

    <span data-ttu-id="09075-127">Pokud konfigurace adresáře CollectD nachází ve /etc/collectd.d/:</span><span class="sxs-lookup"><span data-stu-id="09075-127">If your CollectD config directory is located in /etc/collectd.d/:</span></span>

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/oms.conf /etc/collectd.d/oms.conf

    <span data-ttu-id="09075-128">Pokud konfigurace adresáře CollectD nachází ve /etc/collectd/collectd.conf.d/:</span><span class="sxs-lookup"><span data-stu-id="09075-128">If your CollectD config directory is located in /etc/collectd/collectd.conf.d/:</span></span>

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/oms.conf /etc/collectd/collectd.conf.d/oms.conf

    >[!NOTE]
    ><span data-ttu-id="09075-129">Pro verze CollectD před 5.5 budete muset upravit značky v `oms.conf` jako v příkladu nahoře.</span><span class="sxs-lookup"><span data-stu-id="09075-129">For CollectD versions before 5.5 you will have to modify the tags in `oms.conf` as shown above.</span></span>
    >

2. <span data-ttu-id="09075-130">Zkopírujte collectd.conf požadované prostoru omsagent konfiguračního adresáře.</span><span class="sxs-lookup"><span data-stu-id="09075-130">Copy collectd.conf to the desired workspace's omsagent configuration directory.</span></span>

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/collectd.conf /etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/
        sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/collectd.conf

3. <span data-ttu-id="09075-131">Restartujte CollectD a OMS agenta pro Linux pomocí následujících příkazů.</span><span class="sxs-lookup"><span data-stu-id="09075-131">Restart CollectD and OMS Agent for Linux with the following commands.</span></span>

    <span data-ttu-id="09075-132">sudo služby collectd restartování sudo /opt/microsoft/omsagent/bin/service_control restartování</span><span class="sxs-lookup"><span data-stu-id="09075-132">sudo service collectd restart  sudo /opt/microsoft/omsagent/bin/service_control restart</span></span>

## <a name="collectd-metrics-to-log-analytics-schema-conversion"></a><span data-ttu-id="09075-133">CollectD metrik převod schématu analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="09075-133">CollectD metrics to Log Analytics schema conversion</span></span>
<span data-ttu-id="09075-134">Chcete-li zachovat, že známý model mezi infrastruktura metriky již shromážděné agentem OMS pro Linux a nové metriky shromažďují CollectD následující schéma mapování se používá:</span><span class="sxs-lookup"><span data-stu-id="09075-134">To maintain a familiar model between infrastructure metrics already collected by OMS Agent for Linux and the new metrics collected by CollectD the following schema mapping is used:</span></span>

| <span data-ttu-id="09075-135">Metrika CollectD pole</span><span class="sxs-lookup"><span data-stu-id="09075-135">CollectD Metric field</span></span> | <span data-ttu-id="09075-136">Pole analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="09075-136">Log Analytics field</span></span> |
|:--|:--|
| <span data-ttu-id="09075-137">hostitele</span><span class="sxs-lookup"><span data-stu-id="09075-137">host</span></span> | <span data-ttu-id="09075-138">Počítač</span><span class="sxs-lookup"><span data-stu-id="09075-138">Computer</span></span> |
| <span data-ttu-id="09075-139">Modul plug-in</span><span class="sxs-lookup"><span data-stu-id="09075-139">plugin</span></span> | <span data-ttu-id="09075-140">Žádný</span><span class="sxs-lookup"><span data-stu-id="09075-140">None</span></span> |
| <span data-ttu-id="09075-141">plugin_instance</span><span class="sxs-lookup"><span data-stu-id="09075-141">plugin_instance</span></span> | <span data-ttu-id="09075-142">Název instance</span><span class="sxs-lookup"><span data-stu-id="09075-142">Instance Name</span></span><br><span data-ttu-id="09075-143">Pokud **plugin_instance** je *null* pak InstanceName = "*_celkem*"</span><span class="sxs-lookup"><span data-stu-id="09075-143">If **plugin_instance** is *null* then InstanceName="*_Total*"</span></span> |
| <span data-ttu-id="09075-144">type</span><span class="sxs-lookup"><span data-stu-id="09075-144">type</span></span> | <span data-ttu-id="09075-145">Název objektu</span><span class="sxs-lookup"><span data-stu-id="09075-145">ObjectName</span></span> |
| <span data-ttu-id="09075-146">type_instance</span><span class="sxs-lookup"><span data-stu-id="09075-146">type_instance</span></span> | <span data-ttu-id="09075-147">Název_čítače</span><span class="sxs-lookup"><span data-stu-id="09075-147">CounterName</span></span><br><span data-ttu-id="09075-148">Pokud **type_instance** je *null* pak CounterName =**prázdné**</span><span class="sxs-lookup"><span data-stu-id="09075-148">If **type_instance** is *null* then CounterName=**blank**</span></span> |
| <span data-ttu-id="09075-149">[dsnames]</span><span class="sxs-lookup"><span data-stu-id="09075-149">dsnames[]</span></span> | <span data-ttu-id="09075-150">Název_čítače</span><span class="sxs-lookup"><span data-stu-id="09075-150">CounterName</span></span> |
| <span data-ttu-id="09075-151">dstypes</span><span class="sxs-lookup"><span data-stu-id="09075-151">dstypes</span></span> | <span data-ttu-id="09075-152">Žádný</span><span class="sxs-lookup"><span data-stu-id="09075-152">None</span></span> |
| <span data-ttu-id="09075-153">[] – hodnoty</span><span class="sxs-lookup"><span data-stu-id="09075-153">values[]</span></span> | <span data-ttu-id="09075-154">Přepočtené</span><span class="sxs-lookup"><span data-stu-id="09075-154">CounterValue</span></span> |

## <a name="next-steps"></a><span data-ttu-id="09075-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="09075-155">Next steps</span></span>
* <span data-ttu-id="09075-156">Další informace o [protokolu hledání](log-analytics-log-searches.md) analyzovat data shromážděná ze zdrojů dat a řešení.</span><span class="sxs-lookup"><span data-stu-id="09075-156">Learn about [log searches](log-analytics-log-searches.md) to analyze the data collected from data sources and solutions.</span></span> 
* <span data-ttu-id="09075-157">Použití [vlastní pole](log-analytics-custom-fields.md) k analýze dat z syslog záznamů do jednotlivých polí.</span><span class="sxs-lookup"><span data-stu-id="09075-157">Use [Custom Fields](log-analytics-custom-fields.md) to parse data from syslog records into individual fields.</span></span>

