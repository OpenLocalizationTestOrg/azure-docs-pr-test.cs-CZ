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
# <a name="monitor-an-azure-container-service-cluster-with-elk"></a><span data-ttu-id="62393-104">Monitorování clusteru Azure Container Service s ELK</span><span class="sxs-lookup"><span data-stu-id="62393-104">Monitor an Azure Container Service cluster with ELK</span></span>
<span data-ttu-id="62393-105">V tomto článku jsme ukazují, jak toodeploy hello ELK (Elasticsearch, Logstash, Kibana) zásobníku na cluster DC/OS v Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="62393-105">In this article, we demonstrate how toodeploy hello ELK (Elasticsearch, Logstash, Kibana) stack on a DC/OS cluster in Azure Container Service.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="62393-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="62393-106">Prerequisites</span></span>
<span data-ttu-id="62393-107">[Nasazení](container-service-deployment.md) a [připojit](../container-service-connect.md) cluster DC/OS nakonfigurovaný v Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="62393-107">[Deploy](container-service-deployment.md) and [connect](../container-service-connect.md) a DC/OS cluster configured by Azure Container Service.</span></span> <span data-ttu-id="62393-108">Prozkoumejte řídicí panel hello DC/OS a Marathonu služby [zde](container-service-mesos-marathon-ui.md).</span><span class="sxs-lookup"><span data-stu-id="62393-108">Explore hello DC/OS dashboard and Marathon services [here](container-service-mesos-marathon-ui.md).</span></span> <span data-ttu-id="62393-109">Také nainstalovat hello [nástroj pro vyrovnávání zatížení Marathon](container-service-load-balancing.md).</span><span class="sxs-lookup"><span data-stu-id="62393-109">Also install hello [Marathon Load Balancer](container-service-load-balancing.md).</span></span>


## <a name="elk-elasticsearch-logstash-kibana"></a><span data-ttu-id="62393-110">ELK (Elasticsearch, Logstash, Kibana)</span><span class="sxs-lookup"><span data-stu-id="62393-110">ELK (Elasticsearch, Logstash, Kibana)</span></span>
<span data-ttu-id="62393-111">Zásobník ELK je kombinací Elasticsearch, Logstash, a Kibana poskytující zásobníku tooend end, který lze použít toomonitor a analýza protokolů v clusteru.</span><span class="sxs-lookup"><span data-stu-id="62393-111">ELK stack is a combination of Elasticsearch, Logstash, and Kibana that provides an end tooend stack that can be used toomonitor and analyze logs in your cluster.</span></span>

## <a name="configure-hello-elk-stack-on-a-dcos-cluster"></a><span data-ttu-id="62393-112">Konfigurace hello ELK zásobníku na cluster DC/OS</span><span class="sxs-lookup"><span data-stu-id="62393-112">Configure hello ELK stack on a DC/OS cluster</span></span>
<span data-ttu-id="62393-113">Přístup přes uživatelské rozhraní DC/OS [http://localhost:80 /](http://localhost:80/) jednou v hello uživatelského rozhraní DC/OS přejděte příliš**Universe**.</span><span class="sxs-lookup"><span data-stu-id="62393-113">Access your DC/OS UI via [http://localhost:80/](http://localhost:80/) Once in hello DC/OS UI navigate too**Universe**.</span></span> <span data-ttu-id="62393-114">Vyhledávat a instalovat Elasticsearch, Logstash a Kibana hello Universe DC/OS a v tomto konkrétní pořadí.</span><span class="sxs-lookup"><span data-stu-id="62393-114">Search and install Elasticsearch, Logstash, and Kibana from hello DC/OS Universe and in that specific order.</span></span> <span data-ttu-id="62393-115">Další informace o konfiguraci Pokud přejdete toohello **rozšířený instalace** odkaz.</span><span class="sxs-lookup"><span data-stu-id="62393-115">You can learn more about configuration if you go toohello **Advanced Installation** link.</span></span>

![ELK1](./media/container-service-monitoring-elk/elk1.PNG) ![ELK2](./media/container-service-monitoring-elk/elk2.PNG) ![ELK3](./media/container-service-monitoring-elk/elk3.PNG) 

<span data-ttu-id="62393-119">Jednou hello ELK kontejnery a jsou fungovaly, musíte toobe Kibana tooenable přistupovat prostřednictvím Marathon-LB.</span><span class="sxs-lookup"><span data-stu-id="62393-119">Once hello ELK containers and are up and running, you need tooenable Kibana toobe accessed through Marathon-LB.</span></span> <span data-ttu-id="62393-120">Přejděte příliš **služby** > **kibana**a klikněte na tlačítko **upravit** jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="62393-120">Navigate too **Services** > **kibana**, and click **Edit** as shown below.</span></span>

![ELK4](./media/container-service-monitoring-elk/elk4.PNG)


<span data-ttu-id="62393-122">Přepněte příliš**režim JSON** a přejděte dolů toohello popisky části.</span><span class="sxs-lookup"><span data-stu-id="62393-122">Toggle too**JSON mode** and scroll down toohello labels section.</span></span>
<span data-ttu-id="62393-123">Je třeba tooadd `"HAPROXY_GROUP": "external"` položka zde jak je uvedené níže.</span><span class="sxs-lookup"><span data-stu-id="62393-123">You need tooadd a `"HAPROXY_GROUP": "external"` entry here as shown below.</span></span>
<span data-ttu-id="62393-124">Po kliknutí na tlačítko **nasadit změny**, restartování kontejneru.</span><span class="sxs-lookup"><span data-stu-id="62393-124">Once you click **Deploy changes**, your container restarts.</span></span>

![ELK5](./media/container-service-monitoring-elk/elk5.PNG)


<span data-ttu-id="62393-126">Pokud chcete, aby tooverify této Kibana je zaregistrován jako služba hello HAPROXY řídicím panelu, musíte tooopen port 9090 v clusteru agenta hello HAPROXY běží na port 9090.</span><span class="sxs-lookup"><span data-stu-id="62393-126">If you want tooverify that Kibana is registered as a service in hello HAPROXY dashboard, you need tooopen port 9090 on hello agent cluster as HAPROXY runs on port 9090.</span></span>
<span data-ttu-id="62393-127">Ve výchozím nastavení jsme otevřít porty 80, 8080 a 443 v hello agenta clusteru DC/OS.</span><span class="sxs-lookup"><span data-stu-id="62393-127">By default, we open ports 80, 8080, and 443 in hello DC/OS agent cluster.</span></span>
<span data-ttu-id="62393-128">Pokyny tooopen port a zadejte veřejné vyhodnocení jsou k dispozici [zde](container-service-enable-public-access.md).</span><span class="sxs-lookup"><span data-stu-id="62393-128">Instructions tooopen a port and provide public assess are provided [here](container-service-enable-public-access.md).</span></span>

<span data-ttu-id="62393-129">tooaccess hello HAPROXY řídicího panelu, otevřete hello Marathon-LB rozhraní správce v: `http://$PUBLIC_NODE_IP_ADDRESS:9090/haproxy?stats`.</span><span class="sxs-lookup"><span data-stu-id="62393-129">tooaccess hello HAPROXY dashboard, open hello Marathon-LB admin interface at: `http://$PUBLIC_NODE_IP_ADDRESS:9090/haproxy?stats`.</span></span>
<span data-ttu-id="62393-130">Až přejdete na adresu URL toohello, jak je uvedeno dále byste měli vidět řídicí panel HAPROXY hello a měli byste vidět položku služby pro Kibana.</span><span class="sxs-lookup"><span data-stu-id="62393-130">Once you navigate toohello URL, you should see hello HAPROXY dashboard as shown below and you should see a service entry for Kibana.</span></span>

![ELK6](./media/container-service-monitoring-elk/elk6.PNG)


<span data-ttu-id="62393-132">řídicí panel hello Kibana tooaccess, který je nasazen na portu 5601, potřebujete tooopen port 5601.</span><span class="sxs-lookup"><span data-stu-id="62393-132">tooaccess hello Kibana dashboard, which is deployed on port 5601, you need tooopen port 5601.</span></span> <span data-ttu-id="62393-133">Postupujte podle pokynů [zde](container-service-enable-public-access.md).</span><span class="sxs-lookup"><span data-stu-id="62393-133">Follow instructions [here](container-service-enable-public-access.md).</span></span> <span data-ttu-id="62393-134">Otevřete řídicí panel Kibana hello v: `http://localhost:5601`.</span><span class="sxs-lookup"><span data-stu-id="62393-134">Then open hello Kibana dashboard at: `http://localhost:5601`.</span></span>

## <a name="next-steps"></a><span data-ttu-id="62393-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="62393-135">Next steps</span></span>

* <span data-ttu-id="62393-136">Pro systémové a aplikační protokol předávání a nastavení, najdete v části [Správa protokolu v DC/OS s ELK](https://docs.mesosphere.com/1.8/administration/logging/elk/).</span><span class="sxs-lookup"><span data-stu-id="62393-136">For system and application log forwarding and setup, see [Log Management in DC/OS with ELK](https://docs.mesosphere.com/1.8/administration/logging/elk/).</span></span>

* <span data-ttu-id="62393-137">najdete v části protokoly toofilter [filtrování protokoly s ELK](https://docs.mesosphere.com/1.8/administration/logging/filter-elk/).</span><span class="sxs-lookup"><span data-stu-id="62393-137">toofilter logs, see [Filtering Logs with ELK](https://docs.mesosphere.com/1.8/administration/logging/filter-elk/).</span></span> 

 

