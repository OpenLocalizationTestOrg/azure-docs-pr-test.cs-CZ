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
# <a name="collect-data-from-collectd-on-linux-agents-in-log-analytics"></a>Shromažďování dat z CollectD na agentech Linux v analýzy protokolů
[CollectD](https://collectd.org/) je démon Linux s otevřeným zdrojem, který pravidelně shromažďuje metriky výkonu z aplikace a informace o úrovni systému. Příklad aplikace zahrnovat hello Java Virtual Machine (JVM), MySQL Server a Nginx. Tento článek obsahuje informace o shromažďování dat výkonu z CollectD v analýzy protokolů.

Úplný seznam dostupných modulů plug-in najdete na [tabulky modulů plug-in](https://collectd.org/wiki/index.php/Table_of_Plugins).

![Přehled CollectD](media/log-analytics-data-sources-collectd/overview.png)

Hello následující konfiguraci CollectD je součástí hello OMS agenta pro Linux tooroute CollectD data toohello OMS agenta pro Linux.

    LoadPlugin write_http

    <Plugin write_http>
         <Node "oms">
         URL "127.0.0.1:26000/oms.collectd"
         Format "JSON"
         StoreRates true
         </Node>
    </Plugin>

Kromě toho pokud se používá verzích collectD než 5.5 použít hello místo následující konfigurace.

    LoadPlugin write_http

    <Plugin write_http>
       <URL "127.0.0.1:26000/oms.collectd">
        Format "JSON"
         StoreRates true
       </URL>
    </Plugin>

Konfigurace CollectD Hello používá výchozí hello`write_http` metriky dat výkonu modulu plug-in toosend přes port 26000 tooOMS agenta pro Linux. 

> [!NOTE]
> Tento port může být nakonfigurované tooa vlastní port, v případě potřeby.

Hello OMS agenta pro Linux také naslouchá na portu 26000 CollectD metrik a převede je tooOMS schématu metriky. Hello následuje hello OMS agenta pro Linux konfiguraci `collectd.conf`.

    <source>
      type http
      port 26000
      bind 127.0.0.1
    </source>

    <filter oms.collectd>
      type filter_collectd
    </filter>


## <a name="versions-supported"></a>Podporované verze
- Aktuálně podporuje CollectD verze 4.8 analýzy protokolů a vyšší.
- Je vyžadována pro kolekci metriky CollectD OMS agenta pro Linux v1.1.0-217 nebo vyšší.


## <a name="configuration"></a>Konfigurace
Hello následují základní kroky tooconfigure kolekce dat CollectD v analýzy protokolů.

1. Nakonfigurujte CollectD toosend data toohello OMS agenta pro Linux pomocí modulu plug-in write_http hello.  
2. Nakonfigurujte hello OMS agenta pro Linux toolisten pro hello CollectD data na příslušný port hello.
3. Restartujte CollectD a OMS agenta pro Linux.

### <a name="configure-collectd-tooforward-data"></a>Konfigurace CollectD tooforward dat 

1. tooroute CollectD data toohello OMS agenta pro Linux, `oms.conf` toobe musí přidat na tooCollectD konfiguračního adresáře. Cílový Hello tohoto souboru závisí na hello Linux distro počítače.

    Pokud konfigurace adresáře CollectD nachází ve /etc/collectd.d/:

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/oms.conf /etc/collectd.d/oms.conf

    Pokud konfigurace adresáře CollectD nachází ve /etc/collectd/collectd.conf.d/:

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/oms.conf /etc/collectd/collectd.conf.d/oms.conf

    >[!NOTE]
    >Verze CollectD před 5.5 bude mít toomodify hello značky `oms.conf` jako v příkladu nahoře.
    >

2. Zkopírujte prostoru toohello potřeby collectd.conf omsagent konfiguračního adresáře.

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/collectd.conf /etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/
        sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/collectd.conf

3. Restartujte CollectD a OMS agenta pro Linux s hello následující příkazy.

    sudo služby collectd restartování sudo /opt/microsoft/omsagent/bin/service_control restartování

## <a name="collectd-metrics-toolog-analytics-schema-conversion"></a>CollectD metriky tooLog Analytics schématu převod
toomaintain známý model mezi infrastruktura metriky již shromážděné agentem OMS pro systémy Linux a hello nové metriky shromážděné CollectD hello následující schéma mapování se používá:

| Metrika CollectD pole | Pole analýzy protokolů |
|:--|:--|
| hostitele | Počítač |
| Modul plug-in | Žádný |
| plugin_instance | Název instance<br>Pokud **plugin_instance** je *null* pak InstanceName = "*_celkem*" |
| type | Název objektu |
| type_instance | Název_čítače<br>Pokud **type_instance** je *null* pak CounterName =**prázdné** |
| [dsnames] | Název_čítače |
| dstypes | Žádný |
| [] – hodnoty | Přepočtené |

## <a name="next-steps"></a>Další kroky
* Další informace o [protokolu hledání](log-analytics-log-searches.md) tooanalyze hello data budou shromažďovat ze zdroje dat a řešení. 
* Použití [vlastní pole](log-analytics-custom-fields.md) tooparse data z syslog záznamů do jednotlivých polí.

