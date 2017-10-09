---
title: "aaaCollecting vlastní data JSON v OMS Log Analytics | Microsoft Docs"
description: "Vlastní zdroje dat JSON je možné sbírat do analýzy protokolů pomocí hello OMS agenta pro Linux.  Tyto zdroje dat vlastní může být jednoduché skripty vrácení JSON například curl nebo jeden z FluentD na 300 + modulů plug-in. Tento článek popisuje hello konfigurace požadovaná pro tuto kolekci data."
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
ms.openlocfilehash: 97d401408a8c206d4a9ef2ec9b13ba1ca6b5e92b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="collecting-custom-json-data-sources-with-hello-oms-agent-for-linux-in-log-analytics"></a><span data-ttu-id="bb0d8-105">Shromažďování vlastní zdroje dat JSON s hello OMS agenta pro Linux v analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="bb0d8-105">Collecting custom JSON data sources with hello OMS Agent for Linux in Log Analytics</span></span>
<span data-ttu-id="bb0d8-106">Vlastní zdroje dat JSON je možné sbírat do analýzy protokolů pomocí hello OMS agenta pro Linux.</span><span class="sxs-lookup"><span data-stu-id="bb0d8-106">Custom JSON data sources can be collected into Log Analytics using hello OMS Agent for Linux.</span></span>  <span data-ttu-id="bb0d8-107">Tyto zdroje dat vlastní může být jednoduché skripty, jako vrácení JSON [curl](https://curl.haxx.se/) nebo jeden z [FluentD na 300 + modulů plug-in](http://www.fluentd.org/plugins/all).</span><span class="sxs-lookup"><span data-stu-id="bb0d8-107">These custom data sources can be simple scripts returning JSON such as [curl](https://curl.haxx.se/) or one of [FluentD's 300+ plugins](http://www.fluentd.org/plugins/all).</span></span> <span data-ttu-id="bb0d8-108">Tento článek popisuje hello konfigurace požadovaná pro tuto kolekci data.</span><span class="sxs-lookup"><span data-stu-id="bb0d8-108">This article describes hello configuration required for this data collection.</span></span>

> [!NOTE]
> <span data-ttu-id="bb0d8-109">OMS agenta pro Linux v1.1.0-217 + je vyžadována pro vlastní Data JSON</span><span class="sxs-lookup"><span data-stu-id="bb0d8-109">OMS Agent for Linux v1.1.0-217+ is required for Custom JSON Data</span></span>

## <a name="configuration"></a><span data-ttu-id="bb0d8-110">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="bb0d8-110">Configuration</span></span>

### <a name="configure-input-plugin"></a><span data-ttu-id="bb0d8-111">Konfigurace vstupní modulu plug-in</span><span class="sxs-lookup"><span data-stu-id="bb0d8-111">Configure input plugin</span></span>

<span data-ttu-id="bb0d8-112">Přidat toocollect data JSON v analýzy protokolů `oms.api.` toohello začátek FluentD tag vstupní modulu plug-in.</span><span class="sxs-lookup"><span data-stu-id="bb0d8-112">toocollect JSON data in Log Analytics, add `oms.api.` toohello start of a FluentD tag in an input plugin.</span></span>

<span data-ttu-id="bb0d8-113">Například následující je jiný konfigurační soubor `exec-json.conf` v `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/`.</span><span class="sxs-lookup"><span data-stu-id="bb0d8-113">For example, following is a separate configuration file `exec-json.conf` in `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/`.</span></span>  <span data-ttu-id="bb0d8-114">Používá modul plug-in FluentD hello `exec` toorun příkaz curl každých 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="bb0d8-114">This uses hello FluentD plugin `exec` toorun a curl command every 30 seconds.</span></span>  <span data-ttu-id="bb0d8-115">modul plug-in výstup JSON hello shromažďuje Hello výstup tohoto příkazu.</span><span class="sxs-lookup"><span data-stu-id="bb0d8-115">hello output from this command is collected by hello JSON output plugin.</span></span>

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
<span data-ttu-id="bb0d8-116">konfigurační soubor Hello přidají pod `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/` bude vyžadovat toohave změnit jeho vlastnictví s hello následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="bb0d8-116">hello configuration file added under `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/` will require toohave its ownership changed with hello following command.</span></span>

`sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/conf/omsagent.d/exec-json.conf`

### <a name="configure-output-plugin"></a><span data-ttu-id="bb0d8-117">Konfigurace modulu plug-in výstup</span><span class="sxs-lookup"><span data-stu-id="bb0d8-117">Configure output plugin</span></span> 
<span data-ttu-id="bb0d8-118">Přidejte následující výstup modulu plug-in toohello hlavní konfigurace v hello `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf` nebo jako jiný konfigurační soubor umístěn v`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/`</span><span class="sxs-lookup"><span data-stu-id="bb0d8-118">Add hello following output plugin configuration toohello main configuration in `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf` or as a separate configuration file placed in `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/`</span></span>

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

### <a name="restart-oms-agent-for-linux"></a><span data-ttu-id="bb0d8-119">Restartujte agenta OMS pro Linux</span><span class="sxs-lookup"><span data-stu-id="bb0d8-119">Restart OMS Agent for Linux</span></span>
<span data-ttu-id="bb0d8-120">Restartujte hello OMS agenta pro Linux službu s hello následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="bb0d8-120">Restart hello OMS Agent for Linux service with hello following command.</span></span>

    sudo /opt/microsoft/omsagent/bin/service_control restart 

## <a name="output"></a><span data-ttu-id="bb0d8-121">Výstup</span><span class="sxs-lookup"><span data-stu-id="bb0d8-121">Output</span></span>
<span data-ttu-id="bb0d8-122">Hello data budou shromážděny v Log Analytics s typem záznamu `<FLUENTD_TAG>_CL`.</span><span class="sxs-lookup"><span data-stu-id="bb0d8-122">hello data will be collected in Log Analytics with a record type of `<FLUENTD_TAG>_CL`.</span></span>

<span data-ttu-id="bb0d8-123">Například hello vlastní značky `tag oms.api.tomcat` v Log Analytics s typem záznamu `tomcat_CL`.</span><span class="sxs-lookup"><span data-stu-id="bb0d8-123">For example, hello custom tag `tag oms.api.tomcat` in Log Analytics with a record type of `tomcat_CL`.</span></span>  <span data-ttu-id="bb0d8-124">S hello následující hledání protokolů může načíst všechny záznamy tohoto typu.</span><span class="sxs-lookup"><span data-stu-id="bb0d8-124">You could retrieve all records of this type with hello following log search.</span></span>

    Type=tomcat_CL

<span data-ttu-id="bb0d8-125">Vnořené data JSON zdroje jsou podporované, ale jsou uloženy na základě nadřazené pole.</span><span class="sxs-lookup"><span data-stu-id="bb0d8-125">Nested JSON data sources are supported, but are indexed based off of parent field.</span></span> <span data-ttu-id="bb0d8-126">Například následující JSON data hello vrácená analýzy protokolů hledání jako `tag_s : "[{ "a":"1", "b":"2" }]`.</span><span class="sxs-lookup"><span data-stu-id="bb0d8-126">For example, hello following JSON data is returned from a Log Analytics search as `tag_s : "[{ "a":"1", "b":"2" }]`.</span></span>

```
{
    "tag": [{
        "a":"1",
        "b":"2"
    }]
}
```


## <a name="next-steps"></a><span data-ttu-id="bb0d8-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bb0d8-127">Next steps</span></span>
* <span data-ttu-id="bb0d8-128">Další informace o [protokolu hledání](log-analytics-log-searches.md) tooanalyze hello data budou shromažďovat ze zdroje dat a řešení.</span><span class="sxs-lookup"><span data-stu-id="bb0d8-128">Learn about [log searches](log-analytics-log-searches.md) tooanalyze hello data collected from data sources and solutions.</span></span> 
 