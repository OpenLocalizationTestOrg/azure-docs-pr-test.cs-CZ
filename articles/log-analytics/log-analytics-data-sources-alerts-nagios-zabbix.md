---
title: "aaaCollect Nagios a Zabbix výstrahy v OMS Log Analytics | Microsoft Docs"
description: "Nagios a Zabbix jsou nástroje pro sledování s otevřeným zdrojem. Vám může shromažďování výstrahy z těchto nástrojů do analýzy protokolů v pořadí tooanalyze je spolu s výstrahy z jiných zdrojů.  Tento článek popisuje, jak tooconfigure hello OMS agenta pro Linux toocollect výstrahy z těchto systémů."
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
ms.openlocfilehash: 23e2252e4fed8bc87baec063694a8472ca84220d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="collect-alerts-from-nagios-and-zabbix-in-log-analytics-from-oms-agent-for-linux"></a>Shromažďovat výstrahy z Nagios a Zabbix v analýzy protokolů z OMS agenta pro Linux 
[Nagios](https://www.nagios.org/) a [Zabbix](http://www.zabbix.com/) jsou nástroje pro sledování s otevřeným zdrojem.  Vám může shromáždit výstrahy z těchto nástrojů do analýzy protokolů v tooanalyze pořadí je spolu s [výstrahy z jiných zdrojů](log-analytics-alerts.md).  Tento článek popisuje, jak tooconfigure hello OMS agenta pro Linux toocollect výstrahy z těchto systémů.
 
## <a name="configure-alert-collection"></a>Konfigurace shromažďování výstrah

### <a name="configuring-nagios-alert-collection"></a>Konfigurace shromažďování výstrah Nagios
Proveďte následující kroky na výstrahy toocollect serveru Nagios hello hello.

1. Udělení hello uživatele **omsagent** soubor protokolu Nagios toohello přístup pro čtení (tj. `/var/log/nagios/nagios.log`). Za předpokladu, že soubor nagios.log hello je vlastníkem skupiny hello `nagios`, můžete přidat uživatele hello **omsagent** toohello **nagios** skupiny. 

    sudo "usermod" - a -G nagios omsagent

2.  Upravte konfigurační soubor hello na (`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`). Zajistěte, aby hello následující položky jsou existuje a není komentáři se:  

        <source>  
          type tail  
          #Update path toopoint tooyour nagios.log  
          path /var/log/nagios/nagios.log  
          format none  
          tag oms.nagios  
        </source>  
      
        <filter oms.nagios>  
          type filter_nagios_log  
        </filter>  

3. Restartujte démon omsagent hello

    ```
    sudo sh /opt/microsoft/omsagent/bin/service_control restart
    ```

### <a name="configuring-zabbix-alert-collection"></a>Konfigurace shromažďování výstrah Zabbix
toocollect výstrahy ze serveru Zabbix, budete potřebovat toospecify uživatele a heslo v *text vymažte, pokud*. Toto není ideální, ale doporučujeme vytvořit hello uživatele a udělit oprávnění toomonitor onlu.

Proveďte následující kroky na výstrahy toocollect serveru Nagios hello hello.

1. Upravte konfigurační soubor hello na (`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`). Zkontrolujte, zda hello následující položky jsou existuje a není komentáři limitu.  Změnit toovalues jméno a heslo uživatele hello prostředí Zabbix.

        <source>
         type zabbix_alerts
         run_interval 1m
         tag oms.zabbix
         zabbix_url http://localhost/zabbix/api_jsonrpc.php
         zabbix_username Admin
         zabbix_password zabbix
        </source>

2. Restartujte démon omsagent hello

    sudo dílet /opt/microsoft/omsagent/bin/service_control restartovat


## <a name="alert-records"></a>Výstrahy záznamů
Výstrahy záznamy můžete načíst z Nagios a Zabbix pomocí [protokolu hledání](log-analytics-log-searches.md) v analýzy protokolů.

### <a name="nagios-alert-records"></a>Výstraha Nagios záznamů

Výstrahy mají záznamy shromažďují Nagios **typ** z **výstrahy** a **SourceSystem** z **Nagios**.  V následující tabulce hello mají vlastnosti hello.

| Vlastnost | Popis |
|:--- |:--- |
| Typ |*Výstrahy* |
| SourceSystem |*Nagios* |
| AlertName |Název výstrahy hello. |
| AlertDescription | Popis výstrahy hello. |
| AlertState | Stav služby hello nebo hostitele.<br><br>OK<br>UPOZORNĚNÍ<br>NAHORU<br>DOLŮ |
| Název hostitele | Název hostitele hello, který vytvořili výstrahu hello. |
| PriorityNumber | Úroveň priority výstrahy hello. |
| StateType | Typ Hello stavu hello výstrahy.<br><br>Konfigurace SOFT - problém, který nebyl opakovaným zkontrolováním.<br>PEVNÝ - problém, který byl znovu zkontrolují zadaného počtu opakování.  |
| TimeGenerated |Datum a čas hello výstraha byla vytvořena. |


### <a name="zabbix-alert-records"></a>Výstrahy záznamy Zabbix
Výstrahy mají záznamy shromažďují Zabbix **typ** z **výstrahy** a **SourceSystem** z **Zabbix**.  V následující tabulce hello mají vlastnosti hello.

| Vlastnost | Popis |
|:--- |:--- |
| Typ |*Výstrahy* |
| SourceSystem |*Zabbix* |
| AlertName | Název výstrahy hello. |
| AlertPriority | Závažnost výstrahy hello.<br><br>není klasifikovaný<br>Informace o<br>Upozornění<br>Průměr<br>Vysoká<br>po havárii  |
| AlertState | Stav výstrahy hello.<br><br>0 – stav je až toodate.<br>1 - stav není znám.  |
| AlertTypeNumber | Určuje, zda výstraha může vygenerovat více událostí problém.<br><br>0 – stav je až toodate.<br>1 - stav není znám.    |
| Komentáře | Další poznámky pro výstrahu. |
| Název hostitele | Název hostitele hello, který vytvořili výstrahu hello. |
| PriorityNumber | Hodnota označující závažnost výstrahy hello.<br><br>0 – nezařazených<br>1 - informace<br>2 – upozornění<br>3 – průměr<br>4 – vysoká<br>5 - po havárii |
| TimeGenerated |Datum a čas hello výstraha byla vytvořena. |
| TimeLastModified |Datum a čas hello stav výstrahy hello byl naposled změněn. |


## <a name="next-steps"></a>Další kroky
* Další informace o [výstrahy](log-analytics-alerts.md) v analýzy protokolů.
* Další informace o [protokolu hledání](log-analytics-log-searches.md) tooanalyze hello data budou shromažďovat ze zdroje dat a řešení. 
