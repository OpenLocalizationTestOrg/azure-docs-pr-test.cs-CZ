---
title: "Shromažďování vlastní data JSON v OMS Log Analytics | Microsoft Docs"
description: "Vlastní zdroje dat JSON je možné sbírat do analýzy protokolů pro Linux pomocí agenta OMS.  Tyto zdroje dat vlastní může být jednoduché skripty vrácení JSON například curl nebo jeden z FluentD na 300 + modulů plug-in. Tento článek popisuje konfigurace požadované pro tuto kolekci data."
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
ms.date: 05/04/2017
ms.author: magoedte
ms.openlocfilehash: 800ee1269556e7c2d56fbbf2b497c10509b5c78c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="collecting-custom-json-data-sources-with-the-oms-agent-for-linux-in-log-analytics"></a><span data-ttu-id="31ae0-105">Shromažďování vlastní zdroje dat JSON s agentem OMS pro Linux v analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="31ae0-105">Collecting custom JSON data sources with the OMS Agent for Linux in Log Analytics</span></span>
<span data-ttu-id="31ae0-106">Vlastní zdroje dat JSON je možné sbírat do analýzy protokolů pro Linux pomocí agenta OMS.</span><span class="sxs-lookup"><span data-stu-id="31ae0-106">Custom JSON data sources can be collected into Log Analytics using the OMS Agent for Linux.</span></span>  <span data-ttu-id="31ae0-107">Tyto zdroje dat vlastní může být jednoduché skripty, jako vrácení JSON [curl](https://curl.haxx.se/) nebo jeden z [FluentD na 300 + modulů plug-in](http://www.fluentd.org/plugins/all).</span><span class="sxs-lookup"><span data-stu-id="31ae0-107">These custom data sources can be simple scripts returning JSON such as [curl](https://curl.haxx.se/) or one of [FluentD's 300+ plugins](http://www.fluentd.org/plugins/all).</span></span> <span data-ttu-id="31ae0-108">Tento článek popisuje konfigurace požadované pro tuto kolekci data.</span><span class="sxs-lookup"><span data-stu-id="31ae0-108">This article describes the configuration required for this data collection.</span></span>

> [!NOTE]
> <span data-ttu-id="31ae0-109">OMS agenta pro Linux v1.1.0-217 + je vyžadována pro vlastní Data JSON</span><span class="sxs-lookup"><span data-stu-id="31ae0-109">OMS Agent for Linux v1.1.0-217+ is required for Custom JSON Data</span></span>

## <a name="configuration"></a><span data-ttu-id="31ae0-110">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="31ae0-110">Configuration</span></span>

### <a name="configure-input-plugin"></a><span data-ttu-id="31ae0-111">Konfigurace vstupní modulu plug-in</span><span class="sxs-lookup"><span data-stu-id="31ae0-111">Configure input plugin</span></span>

<span data-ttu-id="31ae0-112">Chcete-li shromažďovat data JSON v analýzy protokolů, přidejte `oms.api.` na začátek FluentD tag vstupní modulu plug-in.</span><span class="sxs-lookup"><span data-stu-id="31ae0-112">To collect JSON data in Log Analytics, add `oms.api.` to the start of a FluentD tag in an input plugin.</span></span>

<span data-ttu-id="31ae0-113">Například následující je jiný konfigurační soubor `exec-json.conf` v `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/`.</span><span class="sxs-lookup"><span data-stu-id="31ae0-113">For example, following is a separate configuration file `exec-json.conf` in `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/`.</span></span>  <span data-ttu-id="31ae0-114">Používá modul plug-in FluentD `exec` spustit příkaz curl každých 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="31ae0-114">This uses the FluentD plugin `exec` to run a curl command every 30 seconds.</span></span>  <span data-ttu-id="31ae0-115">Výstup tohoto příkazu se shromažďují modulem plug-in výstup JSON.</span><span class="sxs-lookup"><span data-stu-id="31ae0-115">The output from this command is collected by the JSON output plugin.</span></span>

```
<source>
  type exec
  command 'curl localhost/json.output'
  format json
  tag oms.api.httpresponse
  run_interval 30s
</source>

<match oms.api.httpresponse>
  type out_oms_api
  log_level info

  buffer_chunk_limit 5m
  buffer_type file
  buffer_path /var/opt/microsoft/omsagent/<workspace id>/state/out_oms_api_httpresponse*.buffer
  buffer_queue_limit 10
  flush_interval 20s
  retry_limit 10
  retry_wait 30s
</match>
```
<span data-ttu-id="31ae0-116">Konfigurační soubor přidají pod `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/` bude nutné mít jeho vlastnictví změnit pomocí následujícího příkazu.</span><span class="sxs-lookup"><span data-stu-id="31ae0-116">The configuration file added under `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/` will require to have its ownership changed with the following command.</span></span>

`sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/conf/omsagent.d/exec-json.conf`

### <a name="configure-output-plugin"></a><span data-ttu-id="31ae0-117">Konfigurace modulu plug-in výstup</span><span class="sxs-lookup"><span data-stu-id="31ae0-117">Configure output plugin</span></span> 
<span data-ttu-id="31ae0-118">Přidejte následující konfigurace modulu plug-in výstup do hlavní konfigurace v nástroji `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf` nebo jako jiný konfigurační soubor umístěn v`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/`</span><span class="sxs-lookup"><span data-stu-id="31ae0-118">Add the following output plugin configuration to the main configuration in `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf` or as a separate configuration file placed in `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/`</span></span>

```
<match oms.api.**>
  type out_oms_api
  log_level info

  buffer_chunk_limit 5m
  buffer_type file
  buffer_path /var/opt/microsoft/omsagent/<workspace id>/state/out_oms_api*.buffer
  buffer_queue_limit 10
  flush_interval 20s
  retry_limit 10
  retry_wait 30s
</match>
```

### <a name="restart-oms-agent-for-linux"></a><span data-ttu-id="31ae0-119">Restartujte agenta OMS pro Linux</span><span class="sxs-lookup"><span data-stu-id="31ae0-119">Restart OMS Agent for Linux</span></span>
<span data-ttu-id="31ae0-120">Restartujte agenta OMS služby Linux pomocí následujícího příkazu.</span><span class="sxs-lookup"><span data-stu-id="31ae0-120">Restart the OMS Agent for Linux service with the following command.</span></span>

    sudo /opt/microsoft/omsagent/bin/service_control restart 

## <a name="output"></a><span data-ttu-id="31ae0-121">Výstup</span><span class="sxs-lookup"><span data-stu-id="31ae0-121">Output</span></span>
<span data-ttu-id="31ae0-122">Data budou shromážděny v Log Analytics s typem záznamu `<FLUENTD_TAG>_CL`.</span><span class="sxs-lookup"><span data-stu-id="31ae0-122">The data will be collected in Log Analytics with a record type of `<FLUENTD_TAG>_CL`.</span></span>

<span data-ttu-id="31ae0-123">Například vlastní značky `tag oms.api.tomcat` v Log Analytics s typem záznamu `tomcat_CL`.</span><span class="sxs-lookup"><span data-stu-id="31ae0-123">For example, the custom tag `tag oms.api.tomcat` in Log Analytics with a record type of `tomcat_CL`.</span></span>  <span data-ttu-id="31ae0-124">Může načíst všechny záznamy tohoto typu s následující hledání protokolů.</span><span class="sxs-lookup"><span data-stu-id="31ae0-124">You could retrieve all records of this type with the following log search.</span></span>

    Type=tomcat_CL

<span data-ttu-id="31ae0-125">Vnořené data JSON zdroje jsou podporované, ale jsou uloženy na základě nadřazené pole.</span><span class="sxs-lookup"><span data-stu-id="31ae0-125">Nested JSON data sources are supported, but are indexed based off of parent field.</span></span> <span data-ttu-id="31ae0-126">Například následující data JSON je vrácen z vyhledávání analýzy protokolů jako `tag_s : "[{ "a":"1", "b":"2" }]`.</span><span class="sxs-lookup"><span data-stu-id="31ae0-126">For example, the following JSON data is returned from a Log Analytics search as `tag_s : "[{ "a":"1", "b":"2" }]`.</span></span>

```
{
    "tag": [{
        "a":"1",
        "b":"2"
    }]
}
```


## <a name="next-steps"></a><span data-ttu-id="31ae0-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="31ae0-127">Next steps</span></span>
* <span data-ttu-id="31ae0-128">Další informace o [protokolu hledání](log-analytics-log-searches.md) analyzovat data shromážděná ze zdrojů dat a řešení.</span><span class="sxs-lookup"><span data-stu-id="31ae0-128">Learn about [log searches](log-analytics-log-searches.md) to analyze the data collected from data sources and solutions.</span></span> 
 