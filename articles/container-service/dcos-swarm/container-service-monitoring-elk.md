---
title: "Monitorování Azure DC/OS cluster - ELK zásobníku | Microsoft Docs"
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
ms.openlocfilehash: fcfa277cdd0f3cebc0fbbb23e771fb23ffbe2ca6
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-an-azure-container-service-cluster-with-elk"></a><span data-ttu-id="fe8b8-104">Monitorování clusteru Azure Container Service s ELK</span><span class="sxs-lookup"><span data-stu-id="fe8b8-104">Monitor an Azure Container Service cluster with ELK</span></span>
<span data-ttu-id="fe8b8-105">V tomto článku jsme ukazují, jak nasadit zásobníku ELK (Elasticsearch, Logstash, Kibana) v clusteru DC/OS v Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="fe8b8-105">In this article, we demonstrate how to deploy the ELK (Elasticsearch, Logstash, Kibana) stack on a DC/OS cluster in Azure Container Service.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="fe8b8-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="fe8b8-106">Prerequisites</span></span>
<span data-ttu-id="fe8b8-107">[Nasazení](container-service-deployment.md) a [připojit](../container-service-connect.md) cluster DC/OS nakonfigurovaný v Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="fe8b8-107">[Deploy](container-service-deployment.md) and [connect](../container-service-connect.md) a DC/OS cluster configured by Azure Container Service.</span></span> <span data-ttu-id="fe8b8-108">Prozkoumejte řídicí panel DC/OS a Marathonu služby [zde](container-service-mesos-marathon-ui.md).</span><span class="sxs-lookup"><span data-stu-id="fe8b8-108">Explore the DC/OS dashboard and Marathon services [here](container-service-mesos-marathon-ui.md).</span></span> <span data-ttu-id="fe8b8-109">Taky nainstalovat [nástroj pro vyrovnávání zatížení Marathon](container-service-load-balancing.md).</span><span class="sxs-lookup"><span data-stu-id="fe8b8-109">Also install the [Marathon Load Balancer](container-service-load-balancing.md).</span></span>


## <a name="elk-elasticsearch-logstash-kibana"></a><span data-ttu-id="fe8b8-110">ELK (Elasticsearch, Logstash, Kibana)</span><span class="sxs-lookup"><span data-stu-id="fe8b8-110">ELK (Elasticsearch, Logstash, Kibana)</span></span>
<span data-ttu-id="fe8b8-111">Zásobník ELK je kombinací Elasticsearch, Logstash a Kibana, která poskytuje koncová zásobníku, který slouží ke sledování a analýza protokolů v clusteru.</span><span class="sxs-lookup"><span data-stu-id="fe8b8-111">ELK stack is a combination of Elasticsearch, Logstash, and Kibana that provides an end to end stack that can be used to monitor and analyze logs in your cluster.</span></span>

## <a name="configure-the-elk-stack-on-a-dcos-cluster"></a><span data-ttu-id="fe8b8-112">Konfigurace zásobníku ELK na cluster DC/OS</span><span class="sxs-lookup"><span data-stu-id="fe8b8-112">Configure the ELK stack on a DC/OS cluster</span></span>
<span data-ttu-id="fe8b8-113">Přístup přes uživatelské rozhraní DC/OS [http://localhost:80 /](http://localhost:80/) jednou v uživatelském rozhraní DC/OS přejděte na **Universe**.</span><span class="sxs-lookup"><span data-stu-id="fe8b8-113">Access your DC/OS UI via [http://localhost:80/](http://localhost:80/) Once in the DC/OS UI navigate to **Universe**.</span></span> <span data-ttu-id="fe8b8-114">Vyhledávat a instalovat Elasticsearch, Logstash a Kibana Universe DC/OS a v tomto konkrétní pořadí.</span><span class="sxs-lookup"><span data-stu-id="fe8b8-114">Search and install Elasticsearch, Logstash, and Kibana from the DC/OS Universe and in that specific order.</span></span> <span data-ttu-id="fe8b8-115">Další informace o konfiguraci Pokud přejdete do **rozšířený instalace** odkaz.</span><span class="sxs-lookup"><span data-stu-id="fe8b8-115">You can learn more about configuration if you go to the **Advanced Installation** link.</span></span>

![ELK1](./media/container-service-monitoring-elk/elk1.PNG) ![ELK2](./media/container-service-monitoring-elk/elk2.PNG) ![ELK3](./media/container-service-monitoring-elk/elk3.PNG) 

<span data-ttu-id="fe8b8-119">Jakmile ELK kontejnery a jsou fungovaly, budete muset povolit Kibana přístup přes Marathon-LB.</span><span class="sxs-lookup"><span data-stu-id="fe8b8-119">Once the ELK containers and are up and running, you need to enable Kibana to be accessed through Marathon-LB.</span></span> <span data-ttu-id="fe8b8-120">Přejděte na **služby** > **kibana**a klikněte na tlačítko **upravit** jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="fe8b8-120">Navigate to **Services** > **kibana**, and click **Edit** as shown below.</span></span>

![ELK4](./media/container-service-monitoring-elk/elk4.PNG)


<span data-ttu-id="fe8b8-122">Přepnutím **režim JSON** a přejděte dolů v části štítky.</span><span class="sxs-lookup"><span data-stu-id="fe8b8-122">Toggle to **JSON mode** and scroll down to the labels section.</span></span>
<span data-ttu-id="fe8b8-123">Je nutné přidat `"HAPROXY_GROUP": "external"` položka zde jak je uvedené níže.</span><span class="sxs-lookup"><span data-stu-id="fe8b8-123">You need to add a `"HAPROXY_GROUP": "external"` entry here as shown below.</span></span>
<span data-ttu-id="fe8b8-124">Po kliknutí na tlačítko **nasadit změny**, restartování kontejneru.</span><span class="sxs-lookup"><span data-stu-id="fe8b8-124">Once you click **Deploy changes**, your container restarts.</span></span>

![ELK5](./media/container-service-monitoring-elk/elk5.PNG)


<span data-ttu-id="fe8b8-126">Pokud chcete ověřit, že Kibana je zaregistrován jako služba v řídicím panelu HAPROXY, budete muset otevřít port 9090 v clusteru agenta HAPROXY běží na port 9090.</span><span class="sxs-lookup"><span data-stu-id="fe8b8-126">If you want to verify that Kibana is registered as a service in the HAPROXY dashboard, you need to open port 9090 on the agent cluster as HAPROXY runs on port 9090.</span></span>
<span data-ttu-id="fe8b8-127">Ve výchozím nastavení, jsme otevřít porty 80, 8080 a 443 v clusteru DC/OS agenta.</span><span class="sxs-lookup"><span data-stu-id="fe8b8-127">By default, we open ports 80, 8080, and 443 in the DC/OS agent cluster.</span></span>
<span data-ttu-id="fe8b8-128">Pokyny k otevření portu a zadejte veřejné vyhodnocení [zde](container-service-enable-public-access.md).</span><span class="sxs-lookup"><span data-stu-id="fe8b8-128">Instructions to open a port and provide public assess are provided [here](container-service-enable-public-access.md).</span></span>

<span data-ttu-id="fe8b8-129">Přístup k řídicímu panelu HAPROXY, otevřete rozhraní Marathon-LB správce v: `http://$PUBLIC_NODE_IP_ADDRESS:9090/haproxy?stats`.</span><span class="sxs-lookup"><span data-stu-id="fe8b8-129">To access the HAPROXY dashboard, open the Marathon-LB admin interface at: `http://$PUBLIC_NODE_IP_ADDRESS:9090/haproxy?stats`.</span></span>
<span data-ttu-id="fe8b8-130">Když přejdete na adresu URL, byste měli vidět řídicí panel HAPROXY, jak je uvedeno níže a měli byste vidět položku služby pro Kibana.</span><span class="sxs-lookup"><span data-stu-id="fe8b8-130">Once you navigate to the URL, you should see the HAPROXY dashboard as shown below and you should see a service entry for Kibana.</span></span>

![ELK6](./media/container-service-monitoring-elk/elk6.PNG)


<span data-ttu-id="fe8b8-132">Pro přístup k řídicímu panelu Kibana, který je nasazen na portu 5601, budete muset otevřít port 5601.</span><span class="sxs-lookup"><span data-stu-id="fe8b8-132">To access the Kibana dashboard, which is deployed on port 5601, you need to open port 5601.</span></span> <span data-ttu-id="fe8b8-133">Postupujte podle pokynů [zde](container-service-enable-public-access.md).</span><span class="sxs-lookup"><span data-stu-id="fe8b8-133">Follow instructions [here](container-service-enable-public-access.md).</span></span> <span data-ttu-id="fe8b8-134">Otevřete řídicí panel Kibana v: `http://localhost:5601`.</span><span class="sxs-lookup"><span data-stu-id="fe8b8-134">Then open the Kibana dashboard at: `http://localhost:5601`.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fe8b8-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fe8b8-135">Next steps</span></span>

* <span data-ttu-id="fe8b8-136">Pro systémové a aplikační protokol předávání a nastavení, najdete v části [Správa protokolu v DC/OS s ELK](https://docs.mesosphere.com/1.8/administration/logging/elk/).</span><span class="sxs-lookup"><span data-stu-id="fe8b8-136">For system and application log forwarding and setup, see [Log Management in DC/OS with ELK](https://docs.mesosphere.com/1.8/administration/logging/elk/).</span></span>

* <span data-ttu-id="fe8b8-137">Filtrování protokolů, najdete v článku [filtrování protokoly s ELK](https://docs.mesosphere.com/1.8/administration/logging/filter-elk/).</span><span class="sxs-lookup"><span data-stu-id="fe8b8-137">To filter logs, see [Filtering Logs with ELK](https://docs.mesosphere.com/1.8/administration/logging/filter-elk/).</span></span> 

 

