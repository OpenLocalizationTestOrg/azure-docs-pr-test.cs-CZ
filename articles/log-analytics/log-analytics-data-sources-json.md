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
# <a name="collecting-custom-json-data-sources-with-hello-oms-agent-for-linux-in-log-analytics"></a>Shromažďování vlastní zdroje dat JSON s hello OMS agenta pro Linux v analýzy protokolů
Vlastní zdroje dat JSON je možné sbírat do analýzy protokolů pomocí hello OMS agenta pro Linux.  Tyto zdroje dat vlastní může být jednoduché skripty, jako vrácení JSON [curl](https://curl.haxx.se/) nebo jeden z [FluentD na 300 + modulů plug-in](http://www.fluentd.org/plugins/all). Tento článek popisuje hello konfigurace požadovaná pro tuto kolekci data.

> [!NOTE]
> OMS agenta pro Linux v1.1.0-217 + je vyžadována pro vlastní Data JSON

## <a name="configuration"></a>Konfigurace

### <a name="configure-input-plugin"></a>Konfigurace vstupní modulu plug-in

Přidat toocollect data JSON v analýzy protokolů `oms.api.` toohello začátek FluentD tag vstupní modulu plug-in.

Například následující je jiný konfigurační soubor `exec-json.conf` v `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/`.  Používá modul plug-in FluentD hello `exec` toorun příkaz curl každých 30 sekund.  modul plug-in výstup JSON hello shromažďuje Hello výstup tohoto příkazu.

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
konfigurační soubor Hello přidají pod `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/` bude vyžadovat toohave změnit jeho vlastnictví s hello následující příkaz.

`sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/conf/omsagent.d/exec-json.conf`

### <a name="configure-output-plugin"></a>Konfigurace modulu plug-in výstup 
Přidejte následující výstup modulu plug-in toohello hlavní konfigurace v hello `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf` nebo jako jiný konfigurační soubor umístěn v`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/`

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

### <a name="restart-oms-agent-for-linux"></a>Restartujte agenta OMS pro Linux
Restartujte hello OMS agenta pro Linux službu s hello následující příkaz.

    sudo /opt/microsoft/omsagent/bin/service_control restart 

## <a name="output"></a>Výstup
Hello data budou shromážděny v Log Analytics s typem záznamu `<FLUENTD_TAG>_CL`.

Například hello vlastní značky `tag oms.api.tomcat` v Log Analytics s typem záznamu `tomcat_CL`.  S hello následující hledání protokolů může načíst všechny záznamy tohoto typu.

    Type=tomcat_CL

Vnořené data JSON zdroje jsou podporované, ale jsou uloženy na základě nadřazené pole. Například následující JSON data hello vrácená analýzy protokolů hledání jako `tag_s : "[{ "a":"1", "b":"2" }]`.

```
{
    "tag": [{
        "a":"1",
        "b":"2"
    }]
}
```


## <a name="next-steps"></a>Další kroky
* Další informace o [protokolu hledání](log-analytics-log-searches.md) tooanalyze hello data budou shromažďovat ze zdroje dat a řešení. 
 