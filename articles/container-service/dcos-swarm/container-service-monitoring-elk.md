---
title: "aaaMonitor Azure DC/OS cluster - ELK zásobníku | Microsoft Docs"
description: "Monitorování cluster DC/OS v clusteru Azure Container Service s ELK (Elasticsearch, Logstash a Kibana)."
services: container-service
documentationcenter: 
author: sauryadas
manager: madhana
editor: 
tags: acs, azure-container-service
keywords: "Kontejnery, DC/OS, Azure, monitorování, elk"
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2017
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: 8d81c5342616ec14880d38803cdf95f5845a669b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-an-azure-container-service-cluster-with-elk"></a>Monitorování clusteru Azure Container Service s ELK
V tomto článku jsme ukazují, jak toodeploy hello ELK (Elasticsearch, Logstash, Kibana) zásobníku na cluster DC/OS v Azure Container Service. 

## <a name="prerequisites"></a>Požadavky
[Nasazení](container-service-deployment.md) a [připojit](../container-service-connect.md) cluster DC/OS nakonfigurovaný v Azure Container Service. Prozkoumejte řídicí panel hello DC/OS a Marathonu služby [zde](container-service-mesos-marathon-ui.md). Také nainstalovat hello [nástroj pro vyrovnávání zatížení Marathon](container-service-load-balancing.md).


## <a name="elk-elasticsearch-logstash-kibana"></a>ELK (Elasticsearch, Logstash, Kibana)
Zásobník ELK je kombinací Elasticsearch, Logstash, a Kibana poskytující zásobníku tooend end, který lze použít toomonitor a analýza protokolů v clusteru.

## <a name="configure-hello-elk-stack-on-a-dcos-cluster"></a>Konfigurace hello ELK zásobníku na cluster DC/OS
Přístup přes uživatelské rozhraní DC/OS [http://localhost:80 /](http://localhost:80/) jednou v hello uživatelského rozhraní DC/OS přejděte příliš**Universe**. Vyhledávat a instalovat Elasticsearch, Logstash a Kibana hello Universe DC/OS a v tomto konkrétní pořadí. Další informace o konfiguraci Pokud přejdete toohello **rozšířený instalace** odkaz.

![ELK1](./media/container-service-monitoring-elk/elk1.PNG) ![ELK2](./media/container-service-monitoring-elk/elk2.PNG) ![ELK3](./media/container-service-monitoring-elk/elk3.PNG) 

Jednou hello ELK kontejnery a jsou fungovaly, musíte toobe Kibana tooenable přistupovat prostřednictvím Marathon-LB. Přejděte příliš **služby** > **kibana**a klikněte na tlačítko **upravit** jak je uvedeno níže.

![ELK4](./media/container-service-monitoring-elk/elk4.PNG)


Přepněte příliš**režim JSON** a přejděte dolů toohello popisky části.
Je třeba tooadd `"HAPROXY_GROUP": "external"` položka zde jak je uvedené níže.
Po kliknutí na tlačítko **nasadit změny**, restartování kontejneru.

![ELK5](./media/container-service-monitoring-elk/elk5.PNG)


Pokud chcete, aby tooverify této Kibana je zaregistrován jako služba hello HAPROXY řídicím panelu, musíte tooopen port 9090 v clusteru agenta hello HAPROXY běží na port 9090.
Ve výchozím nastavení jsme otevřít porty 80, 8080 a 443 v hello agenta clusteru DC/OS.
Pokyny tooopen port a zadejte veřejné vyhodnocení jsou k dispozici [zde](container-service-enable-public-access.md).

tooaccess hello HAPROXY řídicího panelu, otevřete hello Marathon-LB rozhraní správce v: `http://$PUBLIC_NODE_IP_ADDRESS:9090/haproxy?stats`.
Až přejdete na adresu URL toohello, jak je uvedeno dále byste měli vidět řídicí panel HAPROXY hello a měli byste vidět položku služby pro Kibana.

![ELK6](./media/container-service-monitoring-elk/elk6.PNG)


řídicí panel hello Kibana tooaccess, který je nasazen na portu 5601, potřebujete tooopen port 5601. Postupujte podle pokynů [zde](container-service-enable-public-access.md). Otevřete řídicí panel Kibana hello v: `http://localhost:5601`.

## <a name="next-steps"></a>Další kroky

* Pro systémové a aplikační protokol předávání a nastavení, najdete v části [Správa protokolu v DC/OS s ELK](https://docs.mesosphere.com/1.8/administration/logging/elk/).

* najdete v části protokoly toofilter [filtrování protokoly s ELK](https://docs.mesosphere.com/1.8/administration/logging/filter-elk/). 

 

