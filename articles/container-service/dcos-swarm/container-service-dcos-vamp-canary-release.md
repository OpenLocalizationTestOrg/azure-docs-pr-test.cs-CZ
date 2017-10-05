---
title: Lesknice verze s Vamp na clusteru Azure DC/OS | Microsoft Docs
description: "Pomocí Vamp lesknice verze služby a použití inteligentního provoz filtrování v clusteru Azure Container Service DC/OS"
services: container-service
author: gggina
manager: rasquill
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 04/17/2017
ms.author: rasquill
ms.custom: mvc
ms.openlocfilehash: 4a20091b59f2643ea71cce99c159a5075706e35d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="canary-release-microservices-with-vamp-on-an-azure-container-service-dcos-cluster"></a><span data-ttu-id="1ae49-103">Lesknice verze mikroslužeb s Vamp v clusteru Azure Container Service DC/OS</span><span class="sxs-lookup"><span data-stu-id="1ae49-103">Canary release microservices with Vamp on an Azure Container Service DC/OS cluster</span></span>

<span data-ttu-id="1ae49-104">V tomto návodu budeme nastavit Vamp v Azure Container Service se cluster DC/OS.</span><span class="sxs-lookup"><span data-stu-id="1ae49-104">In this walkthrough, we set up Vamp on Azure Container Service with a DC/OS cluster.</span></span> <span data-ttu-id="1ae49-105">Jsme lesknice verzi služby ukázku Vamp "Sávy" a potom vyřešte nekompatibility službu s Firefox použitím filtrování inteligentního přenosu.</span><span class="sxs-lookup"><span data-stu-id="1ae49-105">We canary release the Vamp demo service "sava", and then resolve an incompatibility of the service with Firefox by applying smart traffic filtering.</span></span> 

> [!TIP] 
> <span data-ttu-id="1ae49-106">V tomto návodu Vamp běží na clusteru DC/OS, ale můžete také použít Vamp s Kubernetes jako orchestrator.</span><span class="sxs-lookup"><span data-stu-id="1ae49-106">In this walkthrough Vamp runs on a DC/OS cluster, but you can also use Vamp with Kubernetes as the orchestrator.</span></span>
>

## <a name="about-canary-releases-and-vamp"></a><span data-ttu-id="1ae49-107">O Kanárských uvolní a Vamp</span><span class="sxs-lookup"><span data-stu-id="1ae49-107">About canary releases and Vamp</span></span>


<span data-ttu-id="1ae49-108">[Lesknice uvolnění](https://martinfowler.com/bliki/CanaryRelease.html) je přijat inovativní organizacím, jako Netflix, Facebook či Spotify strategie inteligentní nasazení.</span><span class="sxs-lookup"><span data-stu-id="1ae49-108">[Canary releasing](https://martinfowler.com/bliki/CanaryRelease.html) is a smart deployment strategy adopted by innovative organizations like Netflix, Facebook, and Spotify.</span></span> <span data-ttu-id="1ae49-109">Je postup, který dává smysl, protože snižuje problémy, představuje bezpečnostní sítě a zvyšuje inovace.</span><span class="sxs-lookup"><span data-stu-id="1ae49-109">It’s an approach that makes sense, because it reduces issues, introduces safety-nets, and increases innovation.</span></span> <span data-ttu-id="1ae49-110">Proto proč všech společností, které nepoužívají ho?</span><span class="sxs-lookup"><span data-stu-id="1ae49-110">So why aren’t all companies using it?</span></span> <span data-ttu-id="1ae49-111">Rozšíření kanálu CI/CD zahrnout lesknice strategie přidá složitost a vyžaduje devops rozsáhlé znalosti a zkušenosti.</span><span class="sxs-lookup"><span data-stu-id="1ae49-111">Extending a CI/CD pipeline to include canary strategies adds complexity, and requires extensive devops knowledge and experience.</span></span> <span data-ttu-id="1ae49-112">To je dost blokování menší firem a podniků agentem před jejich i začít.</span><span class="sxs-lookup"><span data-stu-id="1ae49-112">That’s enough to block smaller companies and enterprises alike before they even get started.</span></span> 

<span data-ttu-id="1ae49-113">[Vamp](http://vamp.io/) vydává systém open source, který je navržený tak, aby usnadňují tento přechod a převeďte Kanárských funkcí do vašeho scheduleru upřednostňované kontejneru.</span><span class="sxs-lookup"><span data-stu-id="1ae49-113">[Vamp](http://vamp.io/) is an open-source system designed to ease this transition and bring canary releasing features to your preferred container scheduler.</span></span> <span data-ttu-id="1ae49-114">Funkce lesknice na vamp překročí pohledu na základě procenta.</span><span class="sxs-lookup"><span data-stu-id="1ae49-114">Vamp’s canary functionality goes beyond percentage-based rollouts.</span></span> <span data-ttu-id="1ae49-115">Provoz můžete filtrovat a rozdělit na širokou škálu podmínky, například pro cílové konkrétní uživatele, rozsahy IP adres nebo zařízení.</span><span class="sxs-lookup"><span data-stu-id="1ae49-115">Traffic can be filtered and split on a wide range of conditions, for example to target specific users, IP-ranges, or devices.</span></span> <span data-ttu-id="1ae49-116">Vamp sleduje a analyzuje metrik výkonu, povolení pro automatizaci na základě reálného dat.</span><span class="sxs-lookup"><span data-stu-id="1ae49-116">Vamp tracks and analyzes performance metrics, allowing for automation based on real-world data.</span></span> <span data-ttu-id="1ae49-117">Můžete nastavit automatické vrácení zpět na chyby, nebo škálování variant jednotlivých služeb na základě zatížení nebo latence.</span><span class="sxs-lookup"><span data-stu-id="1ae49-117">You can set up automatic rollback on errors, or scale individual service variants based on load or latency.</span></span>

## <a name="set-up-azure-container-service-with-dcos"></a><span data-ttu-id="1ae49-118">Nastavení Azure Container Service s DC/OS</span><span class="sxs-lookup"><span data-stu-id="1ae49-118">Set up Azure Container Service with DC/OS</span></span>



1. <span data-ttu-id="1ae49-119">[Nasadit cluster DC/OS](container-service-deployment.md) se jeden z nich a dva agenti výchozí velikost.</span><span class="sxs-lookup"><span data-stu-id="1ae49-119">[Deploy a DC/OS cluster](container-service-deployment.md) with one master and two agents of default size.</span></span> 

2. <span data-ttu-id="1ae49-120">[Vytvoření tunelu SSH](../container-service-connect.md) se připojit ke clusteru DC/OS.</span><span class="sxs-lookup"><span data-stu-id="1ae49-120">[Create an SSH tunnel](../container-service-connect.md) to connect to the DC/OS cluster.</span></span> <span data-ttu-id="1ae49-121">Tento článek předpokládá můžete tunelového propojení do clusteru v místního portu 80.</span><span class="sxs-lookup"><span data-stu-id="1ae49-121">This article assumes that you tunnel to the cluster on local port 80.</span></span>


## <a name="set-up-vamp"></a><span data-ttu-id="1ae49-122">Nastavit Vamp</span><span class="sxs-lookup"><span data-stu-id="1ae49-122">Set up Vamp</span></span>

<span data-ttu-id="1ae49-123">Teď, když máte spuštěného clusteru DC/OS, můžete nainstalovat Vamp z uživatelského rozhraní DC/OS (http://localhost:80).</span><span class="sxs-lookup"><span data-stu-id="1ae49-123">Now that you have a running DC/OS cluster, you can install Vamp from the DC/OS UI (http://localhost:80).</span></span> 

![Uživatelské rozhraní DC/OS](./media/container-service-dcos-vamp-canary-release/01_set_up_vamp.png)

<span data-ttu-id="1ae49-125">Instalace probíhá ve dvou fázích:</span><span class="sxs-lookup"><span data-stu-id="1ae49-125">Installation is done in two stages:</span></span>

1. <span data-ttu-id="1ae49-126">**Nasazení Elasticsearch**.</span><span class="sxs-lookup"><span data-stu-id="1ae49-126">**Deploy Elasticsearch**.</span></span>

2. <span data-ttu-id="1ae49-127">Potom **nasazení Vamp** instalací balíčku universe Vamp DC/OS.</span><span class="sxs-lookup"><span data-stu-id="1ae49-127">Then **deploy Vamp** by installing the Vamp DC/OS universe package.</span></span>

### <a name="deploy-elasticsearch"></a><span data-ttu-id="1ae49-128">Nasazení Elasticsearch</span><span class="sxs-lookup"><span data-stu-id="1ae49-128">Deploy Elasticsearch</span></span>

<span data-ttu-id="1ae49-129">Vamp vyžaduje Elasticsearch metriky shromažďování a agregace.</span><span class="sxs-lookup"><span data-stu-id="1ae49-129">Vamp requires Elasticsearch for metrics collection and aggregation.</span></span> <span data-ttu-id="1ae49-130">Můžete použít [imagí Dockeru magneticio](https://hub.docker.com/r/magneticio/elastic/) nasazení kompatibilní Vamp Elasticsearch zásobníku.</span><span class="sxs-lookup"><span data-stu-id="1ae49-130">You can use the [magneticio Docker images](https://hub.docker.com/r/magneticio/elastic/) to deploy a compatible Vamp Elasticsearch stack.</span></span>

1. <span data-ttu-id="1ae49-131">V uživatelském rozhraní DC/OS, přejděte do **služby** a klikněte na tlačítko **nasadit službu**.</span><span class="sxs-lookup"><span data-stu-id="1ae49-131">In the DC/OS UI, go to **Services** and click **Deploy Service**.</span></span>

2. <span data-ttu-id="1ae49-132">Vyberte **režim JSON** z **nasazení nové služby** automaticky otevírané okno.</span><span class="sxs-lookup"><span data-stu-id="1ae49-132">Select **JSON mode** from the **Deploy New Service** pop-up.</span></span>

  ![Vyberte režim JSON](./media/container-service-dcos-vamp-canary-release/02_deploy_service_json_mode.png)

3. <span data-ttu-id="1ae49-134">Vložte následující kód JSON.</span><span class="sxs-lookup"><span data-stu-id="1ae49-134">Paste in the following JSON.</span></span> <span data-ttu-id="1ae49-135">Tato konfigurace používá s 1 GB paměti RAM a kontrola základní stavu na portu Elasticsearch kontejneru.</span><span class="sxs-lookup"><span data-stu-id="1ae49-135">This configuration runs the container with 1 GB of RAM and a basic health check on the Elasticsearch port.</span></span>
  
  ```JSON
  {
    "id": "elasticsearch",
    "instances": 1,
    "cpus": 0.2,
    "mem": 1024.0,
    "container": {
      "docker": {
        "image": "magneticio/elastic:2.2",
        "network": "HOST",
        "forcePullImage": true
      }
    },
    "healthChecks": [
      {
        "protocol": "TCP",
        "gracePeriodSeconds": 30,
        "intervalSeconds": 10,
        "timeoutSeconds": 5,
        "port": 9200,
        "maxConsecutiveFailures": 0
      }
    ]
  }
  ```
  

3. <span data-ttu-id="1ae49-136">Klikněte na tlačítko **nasazení**.</span><span class="sxs-lookup"><span data-stu-id="1ae49-136">Click **Deploy**.</span></span>

  <span data-ttu-id="1ae49-137">DC/OS nasadí Elasticsearch kontejneru.</span><span class="sxs-lookup"><span data-stu-id="1ae49-137">DC/OS deploys the Elasticsearch container.</span></span> <span data-ttu-id="1ae49-138">Můžete sledovat průběh **služby** stránky.</span><span class="sxs-lookup"><span data-stu-id="1ae49-138">You can track progress on the **Services** page.</span></span>  

  ![nasazení e? Elasticsearch](./media/container-service-dcos-vamp-canary-release/03_deply_elasticsearch.png)

### <a name="deploy-vamp"></a><span data-ttu-id="1ae49-140">Nasazení Vamp</span><span class="sxs-lookup"><span data-stu-id="1ae49-140">Deploy Vamp</span></span>

<span data-ttu-id="1ae49-141">Jakmile Elasticsearch sestavy jako **systémem**, můžete přidat balíček Vamp Universe DC/OS.</span><span class="sxs-lookup"><span data-stu-id="1ae49-141">Once Elasticsearch reports as **Running**, you can add the Vamp DC/OS Universe package.</span></span> 

1. <span data-ttu-id="1ae49-142">Přejděte na **Universe** a vyhledejte **vamp**.</span><span class="sxs-lookup"><span data-stu-id="1ae49-142">Go to **Universe** and search for **vamp**.</span></span> 
  <span data-ttu-id="1ae49-143">![Vamp na DC/OS universe](./media/container-service-dcos-vamp-canary-release/04_universe_deploy_vamp.png)</span><span class="sxs-lookup"><span data-stu-id="1ae49-143">![Vamp on DC/OS universe](./media/container-service-dcos-vamp-canary-release/04_universe_deploy_vamp.png)</span></span>

2. <span data-ttu-id="1ae49-144">Klikněte na tlačítko **nainstalovat** vedle vamp balíčku a vyberte **rozšířený instalace**.</span><span class="sxs-lookup"><span data-stu-id="1ae49-144">Click **install** next to the vamp package, and choose **Advanced Installation**.</span></span>

3. <span data-ttu-id="1ae49-145">Posuňte se dolů a zadejte následující adresu url elasticsearch: `http://elasticsearch.marathon.mesos:9200`.</span><span class="sxs-lookup"><span data-stu-id="1ae49-145">Scroll down and enter the following elasticsearch-url: `http://elasticsearch.marathon.mesos:9200`.</span></span> 

  ![Zadejte adresu URL Elasticsearch](./media/container-service-dcos-vamp-canary-release/05_universe_elasticsearch_url.png)

4. <span data-ttu-id="1ae49-147">Klikněte na tlačítko **zkontrolovat a nainstalovat**, pak klikněte na tlačítko **nainstalovat** ke spuštění nasazení.</span><span class="sxs-lookup"><span data-stu-id="1ae49-147">Click **Review and Install**, then click **Install** to start the deployment.</span></span>  

  <span data-ttu-id="1ae49-148">DC/OS nasadí všechny požadované součásti Vamp.</span><span class="sxs-lookup"><span data-stu-id="1ae49-148">DC/OS deploys all required Vamp components.</span></span> <span data-ttu-id="1ae49-149">Můžete sledovat průběh **služby** stránky.</span><span class="sxs-lookup"><span data-stu-id="1ae49-149">You can track progress on the **Services** page.</span></span>
  
  ![Nasazení Vamp jako balíček universe](./media/container-service-dcos-vamp-canary-release/06_deploy_vamp.png)
  
5. <span data-ttu-id="1ae49-151">Po dokončení nasazení se můžete dostat Vamp uživatelského rozhraní:</span><span class="sxs-lookup"><span data-stu-id="1ae49-151">Once deployment has completed, you can access the Vamp UI:</span></span>

  ![Služba vamp na DC/OS](./media/container-service-dcos-vamp-canary-release/07_deploy_vamp_complete.png)
  
  ![Vamp uživatelského rozhraní](./media/container-service-dcos-vamp-canary-release/08_vamp_ui.png)


## <a name="deploy-your-first-service"></a><span data-ttu-id="1ae49-154">Nasazení vaší první službě</span><span class="sxs-lookup"><span data-stu-id="1ae49-154">Deploy your first service</span></span>

<span data-ttu-id="1ae49-155">Nyní tento Vamp je spuštěná, nasazení služby z plán, podle kterého.</span><span class="sxs-lookup"><span data-stu-id="1ae49-155">Now that Vamp is up and running, deploy a service from a blueprint.</span></span> 

<span data-ttu-id="1ae49-156">Ve své nejjednodušší podobě [Vamp plán, podle kterého](http://vamp.io/documentation/using-vamp/blueprints/) popisuje koncových bodů (brány), clustery a služby pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="1ae49-156">In its simplest form, a [Vamp blueprint](http://vamp.io/documentation/using-vamp/blueprints/) describes the endpoints (gateways), clusters, and services to deploy.</span></span> <span data-ttu-id="1ae49-157">Vamp používá clustery k seskupení různé varianty stejnou službu do logických skupin pro lesknice uvolněním nebo A / B testování.</span><span class="sxs-lookup"><span data-stu-id="1ae49-157">Vamp uses clusters to group different variants of the same service into logical groups for canary releasing or A/B testing.</span></span>  

<span data-ttu-id="1ae49-158">Tento scénář používá ukázkovou aplikaci monolitický názvem [ **Sávy**](https://github.com/magneticio/sava), který je ve verzi 1.0.</span><span class="sxs-lookup"><span data-stu-id="1ae49-158">This scenario uses a sample monolithic application called [**sava**](https://github.com/magneticio/sava), which is at version 1.0.</span></span> <span data-ttu-id="1ae49-159">Monolitu je součástí Docker kontejneru, který je v úložiště Docker Hub v rámci magneticio/sava:1.0.0.</span><span class="sxs-lookup"><span data-stu-id="1ae49-159">The monolith is packaged in a Docker container, which is in Docker Hub under magneticio/sava:1.0.0.</span></span> <span data-ttu-id="1ae49-160">Aplikace se za normálních okolností běží na portu 8080, ale chcete v tomto případě vystavit v rámci port 9050.</span><span class="sxs-lookup"><span data-stu-id="1ae49-160">The app normally runs on port 8080, but you want to expose it under port 9050 in this case.</span></span> <span data-ttu-id="1ae49-161">Nasaďte aplikaci prostřednictvím Vamp používá jednoduchý plán, podle kterého.</span><span class="sxs-lookup"><span data-stu-id="1ae49-161">Deploy the app through Vamp using a simple blueprint.</span></span>

1. <span data-ttu-id="1ae49-162">Přejděte na **nasazení**.</span><span class="sxs-lookup"><span data-stu-id="1ae49-162">Go to **Deployments**.</span></span>

2. <span data-ttu-id="1ae49-163">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="1ae49-163">Click **Add**.</span></span>

3. <span data-ttu-id="1ae49-164">Vložte následující matrici YAML.</span><span class="sxs-lookup"><span data-stu-id="1ae49-164">Paste in the following blueprint YAML.</span></span> <span data-ttu-id="1ae49-165">Tento plán, podle kterého obsahuje jeden cluster s variant pouze jedna služba, která nám změnit později:</span><span class="sxs-lookup"><span data-stu-id="1ae49-165">This blueprint contains one cluster with only one service variant, which we change in a later step:</span></span>

  ```YAML
  name: sava                        # deployment name
  gateways:
    9050: sava_cluster/webport      # stable endpoint
  clusters:
    sava_cluster:               # cluster to create
     services:
        -
          breed:
            name: sava:1.0.0        # service variant name
            deployable: magneticio/sava:1.0.0
            ports:
              webport: 8080/http # cluster endpoint, used for canary releasing
  ```

4. <span data-ttu-id="1ae49-166">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="1ae49-166">Click **Save**.</span></span> <span data-ttu-id="1ae49-167">Vamp zahájí nasazení.</span><span class="sxs-lookup"><span data-stu-id="1ae49-167">Vamp initiates the deployment.</span></span>

<span data-ttu-id="1ae49-168">Nasazení je uvedena na **nasazení** stránky.</span><span class="sxs-lookup"><span data-stu-id="1ae49-168">The deployment is listed on the **Deployments** page.</span></span> <span data-ttu-id="1ae49-169">Klikněte na tlačítko nasazení monitorovat jeho stav.</span><span class="sxs-lookup"><span data-stu-id="1ae49-169">Click the deployment to monitor its status.</span></span>

![Uživatelské rozhraní – nasazení Sávy vamp](./media/container-service-dcos-vamp-canary-release/09_sava100.png)

![Služba Sávy v Vamp uživatelského rozhraní](./media/container-service-dcos-vamp-canary-release/09a_sava100.png)

<span data-ttu-id="1ae49-172">Vytváří dvě brány, které jsou uvedeny na **brány** stránky:</span><span class="sxs-lookup"><span data-stu-id="1ae49-172">Two gateways are created, which are listed on the **Gateways** page:</span></span>

* <span data-ttu-id="1ae49-173">koncový bod stabilní přístup ke službě spuštěné (port 9050)</span><span class="sxs-lookup"><span data-stu-id="1ae49-173">a stable endpoint to access the running service (port 9050)</span></span> 
* <span data-ttu-id="1ae49-174">spravované Vamp interní brány (Další informace o této brány se později).</span><span class="sxs-lookup"><span data-stu-id="1ae49-174">a Vamp-managed internal gateway (more on this gateway later).</span></span> 

![Vamp uživatelské rozhraní – Sávy brány](./media/container-service-dcos-vamp-canary-release/10_vamp_sava_gateways.png)

<span data-ttu-id="1ae49-176">Službu Sávy má nyní nasadit, ale protože pro přenos dat do ní ještě není známo nástroje pro vyrovnávání zatížení Azure ho nelze přístup externě.</span><span class="sxs-lookup"><span data-stu-id="1ae49-176">The sava service has now deployed, but you can’t access it externally because the Azure Load Balancer doesn’t know to forward traffic to it yet.</span></span> <span data-ttu-id="1ae49-177">Přístup ke službě, aktualizujte konfiguraci sítí Azure.</span><span class="sxs-lookup"><span data-stu-id="1ae49-177">To access the service, update the Azure networking configuration.</span></span>


## <a name="update-the-azure-network-configuration"></a><span data-ttu-id="1ae49-178">Aktualizace konfiguraci sítě Azure</span><span class="sxs-lookup"><span data-stu-id="1ae49-178">Update the Azure network configuration</span></span>

<span data-ttu-id="1ae49-179">Vamp nasadit službu Sávy na uzlech agenta DC/OS, vystavení stabilní koncový bod na port 9050.</span><span class="sxs-lookup"><span data-stu-id="1ae49-179">Vamp deployed the sava service on the DC/OS agent nodes, exposing a stable endpoint at port 9050.</span></span> <span data-ttu-id="1ae49-180">Přístup ke službě z mimo cluster DC/OS, proveďte následující změny konfigurace sítě Azure ve vašem nasazení clusteru:</span><span class="sxs-lookup"><span data-stu-id="1ae49-180">To access the service from outside the DC/OS cluster, make the following changes to the Azure network configuration in your cluster deployment:</span></span> 

1. <span data-ttu-id="1ae49-181">**Konfigurace pro vyrovnávání zatížení Azure** pro agenty (prostředek s názvem **orchestrátoru DC/OS agenta lb xxxx**) s test stavu a pravidla pro přenos dat na portu 9050 na Sávy instance.</span><span class="sxs-lookup"><span data-stu-id="1ae49-181">**Configure the Azure Load Balancer** for the agents (the resource named **dcos-agent-lb-xxxx**) with a health probe and a rule to forward traffic on port 9050 to the sava instances.</span></span> 

2. <span data-ttu-id="1ae49-182">**Aktualizovat skupinu zabezpečení sítě** pro veřejné agenty (prostředek s názvem **XXXX-agent veřejný nsg-XXXX**) umožňuje přenosy na portu 9050.</span><span class="sxs-lookup"><span data-stu-id="1ae49-182">**Update the network security group** for the public agents (the resource named **XXXX-agent-public-nsg-XXXX**) to allow traffic on port 9050.</span></span>

<span data-ttu-id="1ae49-183">Podrobné pokyny k dokončení těchto úloh pomocí portálu Azure najdete v tématu [povolit veřejný přístup k aplikaci Azure Container Service](container-service-enable-public-access.md).</span><span class="sxs-lookup"><span data-stu-id="1ae49-183">For detailed steps to complete these tasks using the Azure portal, see [Enable public access to an Azure Container Service application](container-service-enable-public-access.md).</span></span> <span data-ttu-id="1ae49-184">Zadejte port 9050 pro všechna nastavení portu.</span><span class="sxs-lookup"><span data-stu-id="1ae49-184">Specify port 9050 for all port settings.</span></span>


<span data-ttu-id="1ae49-185">Po všechno, co byla vytvořena, přejděte do **přehled** okno nástroje pro vyrovnávání zatížení agenta DC/OS (prostředek s názvem **orchestrátoru DC/OS agenta lb xxxx**).</span><span class="sxs-lookup"><span data-stu-id="1ae49-185">Once everything has been created, go to the **Overview** blade of the DC/OS agent load balancer (the resource named **dcos-agent-lb-xxxx**).</span></span> <span data-ttu-id="1ae49-186">Najít **veřejnou IP adresu**a použijte adresu pro přístup k Sávy na portu 9050.</span><span class="sxs-lookup"><span data-stu-id="1ae49-186">Find the **Public IP address**, and use the address to access sava at port 9050.</span></span>

![Portál Azure – get veřejnou IP adresu](./media/container-service-dcos-vamp-canary-release/18_public_ip_address.png)

![Sávy](./media/container-service-dcos-vamp-canary-release/19_sava100.png)


## <a name="run-a-canary-release"></a><span data-ttu-id="1ae49-189">Spustit lesknice verze</span><span class="sxs-lookup"><span data-stu-id="1ae49-189">Run a canary release</span></span>

<span data-ttu-id="1ae49-190">Předpokládejme, že máte novou verzi této aplikace, které chcete lesknice vydání do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="1ae49-190">Suppose you have a new version of this application that you want to canary release into production.</span></span> <span data-ttu-id="1ae49-191">Ji kontejnerizované jako magneticio/sava:1.1.0 a připraveni.</span><span class="sxs-lookup"><span data-stu-id="1ae49-191">You have it containerized as magneticio/sava:1.1.0 and are ready to go.</span></span> <span data-ttu-id="1ae49-192">Vamp umožňuje snadno přidat nové služby spuštěné nasazení.</span><span class="sxs-lookup"><span data-stu-id="1ae49-192">Vamp lets you easily add new services to the running deployment.</span></span> <span data-ttu-id="1ae49-193">Tyto služby "sloučené" nasazení společně se stávající služby v clusteru a přiřazený váhu 0 %.</span><span class="sxs-lookup"><span data-stu-id="1ae49-193">These "merged" services are deployed alongside the existing services in the cluster, and assigned a weight of 0%.</span></span> <span data-ttu-id="1ae49-194">Žádný provoz se směruje na nově sloučený, dokud upravit distribuce přenosů.</span><span class="sxs-lookup"><span data-stu-id="1ae49-194">No traffic is routed to a newly merged service until you adjust the traffic distribution.</span></span> <span data-ttu-id="1ae49-195">Posuvník váhy v uživatelském rozhraní Vamp poskytuje úplnou kontrolu nad distribuce, aby vám umožnil přírůstkové úpravy (lesknice vydání) nebo rychlých vrácení zpět.</span><span class="sxs-lookup"><span data-stu-id="1ae49-195">The weight slider in the Vamp UI gives you complete control over the distribution, allowing for incremental adjustments (canary release) or an instant rollback.</span></span>

### <a name="merge-a-new-service-variant"></a><span data-ttu-id="1ae49-196">Sloučení nové služby variant</span><span class="sxs-lookup"><span data-stu-id="1ae49-196">Merge a new service variant</span></span>

<span data-ttu-id="1ae49-197">Sloučit novou službu Sávy 1.1 s spuštěného nasazení:</span><span class="sxs-lookup"><span data-stu-id="1ae49-197">To merge the new sava 1.1 service with the running deployment:</span></span>

1. <span data-ttu-id="1ae49-198">V uživatelském rozhraní Vamp klikněte na tlačítko **plány vyfotografovat**.</span><span class="sxs-lookup"><span data-stu-id="1ae49-198">In the Vamp UI, click **Blueprints**.</span></span>

2. <span data-ttu-id="1ae49-199">Klikněte na tlačítko **přidat** a vložte následující matrici YAML: Tento plán, podle kterého popisuje hodnotu nové služby typu variant (Sávy: 1.1.0) nasadit v rámci stávajícího clusteru (sava_cluster).</span><span class="sxs-lookup"><span data-stu-id="1ae49-199">Click **Add** and paste in the following blueprint YAML: This blueprint describes a new service variant (sava:1.1.0) to deploy within the existing cluster (sava_cluster).</span></span>

  ```YAML
  name: sava:1.1.0      # blueprint name
  clusters:
    sava_cluster:       # cluster to update
      services:
        -
          breed:
            name: sava:1.1.0    # service variant name
            deployable: magneticio/sava:1.1.0    
            ports:
              webport: 8080/http # cluster endpoint to update
  ```
  
3. <span data-ttu-id="1ae49-200">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="1ae49-200">Click **Save**.</span></span> <span data-ttu-id="1ae49-201">Plán, podle kterého je uložena a uvedené na **plány vyfotografovat** stránky.</span><span class="sxs-lookup"><span data-stu-id="1ae49-201">The blueprint is stored and listed on the **Blueprints** page.</span></span>

4. <span data-ttu-id="1ae49-202">Otevřete nabídku Akce na plán, podle kterého Sávy: 1.1 a klikněte na tlačítko **sloučí se**.</span><span class="sxs-lookup"><span data-stu-id="1ae49-202">Open the action menu on the sava:1.1 blueprint and click **Merge to**.</span></span>

  ![Vamp uživatelské rozhraní – plány](./media/container-service-dcos-vamp-canary-release/20_sava110_mergeto.png)

5. <span data-ttu-id="1ae49-204">Vyberte **Sávy** nasazení a klikněte na tlačítko **sloučení**.</span><span class="sxs-lookup"><span data-stu-id="1ae49-204">Select the **sava** deployment and click **Merge**.</span></span>

  ![Vamp uživatelské rozhraní – sloučení plán, podle kterého nasazení](./media/container-service-dcos-vamp-canary-release/21_sava110_merge.png)

<span data-ttu-id="1ae49-206">Vamp nasadí nové služby varianty Sávy: 1.1.0 popsané v plán, podle kterého spolu s Sávy: 1.0.0 v **sava_cluster** spuštěné nasazení.</span><span class="sxs-lookup"><span data-stu-id="1ae49-206">Vamp deploys the new sava:1.1.0 service variant described in the blueprint alongside sava:1.0.0 in the **sava_cluster** of the running deployment.</span></span> 

![Vamp uživatelské rozhraní – aktualizované Sávy nasazení](./media/container-service-dcos-vamp-canary-release/22_sava_cluster.png)

<span data-ttu-id="1ae49-208">**Sávy/sava_cluster/webport** brány (koncový bod clusteru) je aktualizované taky, přidání do nově nasazené Sávy: 1.1.0 trasu.</span><span class="sxs-lookup"><span data-stu-id="1ae49-208">The **sava/sava_cluster/webport** gateway (the cluster endpoint) is also updated, adding a route to the newly deployed sava:1.1.0.</span></span> <span data-ttu-id="1ae49-209">V tomto okamžiku žádný provoz se směruje zde ( **VÁHY** je nastaven na 0 %).</span><span class="sxs-lookup"><span data-stu-id="1ae49-209">At this point, no traffic is routed here (the **WEIGHT** is set to 0%).</span></span>

![Vamp uživatelské rozhraní – clusteru brány](./media/container-service-dcos-vamp-canary-release/23_sava_cluster_webport.png)

### <a name="canary-release"></a><span data-ttu-id="1ae49-211">Lesknice verze</span><span class="sxs-lookup"><span data-stu-id="1ae49-211">Canary release</span></span>

<span data-ttu-id="1ae49-212">V obou verzích Sávy nasazené ve stejném clusteru upravit distribuce přenosů mezi nimi přesunutím **VÁHY** posuvníku.</span><span class="sxs-lookup"><span data-stu-id="1ae49-212">With both versions of sava deployed in the same cluster, adjust the distribution of traffic between them by moving the **WEIGHT** slider.</span></span>

1. <span data-ttu-id="1ae49-213">Klikněte na tlačítko ![Vamp uživatelské rozhraní – upravit](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) vedle **VÁHY**.</span><span class="sxs-lookup"><span data-stu-id="1ae49-213">Click ![Vamp UI - edit](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) next to **WEIGHT**.</span></span>

2. <span data-ttu-id="1ae49-214">Nastavení distribuce váhy 50 % nebo 50 %, klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="1ae49-214">Set the weight distribution to 50%/50% and click **Save**.</span></span>

  ![Vamp uživatelské rozhraní – brána váhy posuvníku](./media/container-service-dcos-vamp-canary-release/24_sava_cluster_webport_weight.png)

3. <span data-ttu-id="1ae49-216">Přejděte zpět do prohlížeče a aktualizujte stránku Sávy několik vícekrát.</span><span class="sxs-lookup"><span data-stu-id="1ae49-216">Go back to your browser and refresh the sava page a few more times.</span></span> <span data-ttu-id="1ae49-217">Aplikace Sávy přepíná mezi Sávy: 1.0 stránky a stránky Sávy: 1.1.</span><span class="sxs-lookup"><span data-stu-id="1ae49-217">The sava application now switches between a sava:1.0 page and a sava:1.1 page.</span></span>

  ![střídání sava1.0 a sava1.1 služby](./media/container-service-dcos-vamp-canary-release/25_sava_100_101.png)


  > [!NOTE]
  > <span data-ttu-id="1ae49-219">Tato alternace stránky nejvhodnější "Incognito" nebo "Anonymní" režim prohlížeče kvůli ukládání do mezipaměti statické prostředky.</span><span class="sxs-lookup"><span data-stu-id="1ae49-219">This alternation of the page works best with the "Incognito" or “Anonymous” mode of your browser because of the caching of static assets.</span></span>
  >

### <a name="filter-traffic"></a><span data-ttu-id="1ae49-220">Filtrování provozu</span><span class="sxs-lookup"><span data-stu-id="1ae49-220">Filter traffic</span></span>

<span data-ttu-id="1ae49-221">Po nasazení Předpokládejme, že jste zjišťuje nekompatibilitu v Sávy: 1.1.0, že příčiny zobrazit problémy v prohlížečích Firefox.</span><span class="sxs-lookup"><span data-stu-id="1ae49-221">Suppose after deployment that you discovered an incompatibility in sava:1.1.0 that causes display problems in Firefox browsers.</span></span> <span data-ttu-id="1ae49-222">Můžete nastavit Vamp a filtrovat příchozí provoz směrovat, že všichni uživatelé Firefox zpět do známé stabilní Sávy: 1.0.0.</span><span class="sxs-lookup"><span data-stu-id="1ae49-222">You can set Vamp to filter incoming traffic and direct all Firefox users back to the known stable sava:1.0.0.</span></span> <span data-ttu-id="1ae49-223">Tento filtr okamžitě přeloží přerušení pro uživatele, Firefox, zatímco ostatní uživatelé nadále využívat výhod vylepšené Sávy: 1.1.0.</span><span class="sxs-lookup"><span data-stu-id="1ae49-223">This filter instantly resolves the disruption for Firefox users, while everybody else continues to enjoy the benefits of the improved sava:1.1.0.</span></span>

<span data-ttu-id="1ae49-224">Vamp používá **podmínky** k filtrování provozu mezi trasy v bránu.</span><span class="sxs-lookup"><span data-stu-id="1ae49-224">Vamp uses **conditions** to filter traffic between routes in a gateway.</span></span> <span data-ttu-id="1ae49-225">Provoz je nejdřív filtrovat a přesměruje podle podmínky použít pro každý postup.</span><span class="sxs-lookup"><span data-stu-id="1ae49-225">Traffic is first filtered and directed according to the conditions applied to each route.</span></span> <span data-ttu-id="1ae49-226">Veškerý zbývající provoz je distribuován podle váhy nastavení brány.</span><span class="sxs-lookup"><span data-stu-id="1ae49-226">All remaining traffic is distributed according to the gateway weight setting.</span></span>

<span data-ttu-id="1ae49-227">Můžete vytvořit podmínku, kterou chcete filtrovat všechny Firefox uživatele a přesměrování je na staré Sávy: 1.0.0:</span><span class="sxs-lookup"><span data-stu-id="1ae49-227">You can create a condition to filter all Firefox users and direct them to the old sava:1.0.0:</span></span>

1. <span data-ttu-id="1ae49-228">Na Sávy/sava_cluster/webport **brány** klikněte na tlačítko ![Vamp uživatelské rozhraní – upravit](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) přidat **PODMÍNKU** k sava/sava_cluster/sava:1.0.0/webport trasy.</span><span class="sxs-lookup"><span data-stu-id="1ae49-228">On the sava/sava_cluster/webport **Gateways** page, click ![Vamp UI - edit](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) to add a **CONDITION** to the route sava/sava_cluster/sava:1.0.0/webport.</span></span> 

2. <span data-ttu-id="1ae49-229">Zadejte podmínku **uživatelský agent == Firefox** a klikněte na tlačítko ![Vamp uživatelské rozhraní – Uložit](./media/container-service-dcos-vamp-canary-release/vamp_ui_save.png).</span><span class="sxs-lookup"><span data-stu-id="1ae49-229">Enter the condition **user-agent == Firefox** and click ![Vamp UI - save](./media/container-service-dcos-vamp-canary-release/vamp_ui_save.png).</span></span>

  <span data-ttu-id="1ae49-230">Vamp přidá podmínku s výchozí sílu % 0.</span><span class="sxs-lookup"><span data-stu-id="1ae49-230">Vamp adds the condition with a default strength of 0%.</span></span> <span data-ttu-id="1ae49-231">Pokud chcete spustit, filtrování provozu, potřebujete upravit sílu podmínku.</span><span class="sxs-lookup"><span data-stu-id="1ae49-231">To start filtering traffic, you need to adjust the condition strength.</span></span>

3. <span data-ttu-id="1ae49-232">Klikněte na tlačítko ![Vamp uživatelské rozhraní – upravit](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) změnit **SÍLU** použít podmínku.</span><span class="sxs-lookup"><span data-stu-id="1ae49-232">Click ![Vamp UI - edit](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) to change the **STRENGTH** applied to the condition.</span></span>
 
4. <span data-ttu-id="1ae49-233">Nastavte **SÍLU** 100 %, klikněte na tlačítko ![Vamp uživatelské rozhraní – Uložit](./media/container-service-dcos-vamp-canary-release/vamp_ui_save.png) uložit.</span><span class="sxs-lookup"><span data-stu-id="1ae49-233">Set the **STRENGTH** to 100% and click ![Vamp UI - save](./media/container-service-dcos-vamp-canary-release/vamp_ui_save.png) to save.</span></span>

  <span data-ttu-id="1ae49-234">Vamp teď odešle veškerý provoz odpovídajících podmínce (všichni uživatelé Firefox) k Sávy: 1.0.0.</span><span class="sxs-lookup"><span data-stu-id="1ae49-234">Vamp now sends all traffic matching the condition (all Firefox users) to sava:1.0.0.</span></span>

  ![Vamp uživatelského rozhraní – použít podmínku brány](./media/container-service-dcos-vamp-canary-release/26_apply_condition.png)

5. <span data-ttu-id="1ae49-236">Nakonec upravte váhy brány odesílat všechny zbývající provoz (všechny uživatele bez Firefox) do nového Sávy: 1.1.0.</span><span class="sxs-lookup"><span data-stu-id="1ae49-236">Finally, adjust the gateway weight to send all remaining traffic (all non-Firefox users) to the new sava:1.1.0.</span></span> <span data-ttu-id="1ae49-237">Klikněte na tlačítko ![Vamp uživatelské rozhraní – upravit](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) vedle **VÁHY** a nastavte je distribuce váhy, 100 % směřuje na sava/sava_cluster/sava:1.1.0/webport trasy.</span><span class="sxs-lookup"><span data-stu-id="1ae49-237">Click ![Vamp UI - edit](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) next to **WEIGHT** and set the weight distribution so 100% is directed to the route sava/sava_cluster/sava:1.1.0/webport.</span></span>

  <span data-ttu-id="1ae49-238">Veškerý provoz nejsou filtrovány podle podmínky se teď přesměruje nové Sávy: 1.1.0.</span><span class="sxs-lookup"><span data-stu-id="1ae49-238">All traffic not filtered by the condition is now directed to the new sava:1.1.0.</span></span>

6. <span data-ttu-id="1ae49-239">Informace o filtru v akci, otevřete dva různé prohlížeče (jeden Firefox a jeden jiný prohlížeč) a přístup ke službě Sávy z obou.</span><span class="sxs-lookup"><span data-stu-id="1ae49-239">To see the filter in action, open two different browsers (one Firefox and one other browser) and access the sava service from both.</span></span> <span data-ttu-id="1ae49-240">Všechny požadavky Firefox jsou odesílány Sávy: 1.0.0, zatímco všech dalších prohlížečích jsou směrované Sávy: 1.1.0.</span><span class="sxs-lookup"><span data-stu-id="1ae49-240">All Firefox requests are sent to sava:1.0.0, while all other browsers are directed to sava:1.1.0.</span></span>

  ![Vamp uživatelské rozhraní – filtrovat provoz](./media/container-service-dcos-vamp-canary-release/27_filter_traffic.png)

## <a name="summing-up"></a><span data-ttu-id="1ae49-242">Shrnutí</span><span class="sxs-lookup"><span data-stu-id="1ae49-242">Summing up</span></span>

<span data-ttu-id="1ae49-243">Tento článek byl rychlý úvod do Vamp na cluster DC/OS.</span><span class="sxs-lookup"><span data-stu-id="1ae49-243">This article was a quick introduction to Vamp on a DC/OS cluster.</span></span> <span data-ttu-id="1ae49-244">Pro začátek jste získali Vamp a spuštěná na vaše Azure Container Service DC/OS clusteru, nasazení služby s Vamp plán, podle kterého a přístup na adrese zveřejněné koncový bod (brány).</span><span class="sxs-lookup"><span data-stu-id="1ae49-244">For starters, you got Vamp up and running on your Azure Container Service DC/OS cluster, deployed a service with a Vamp blueprint, and accessed it at the exposed endpoint (gateway).</span></span>

<span data-ttu-id="1ae49-245">Můžeme také dotýkal na některé výkonné funkce Vamp: slučování hodnotu typu variant nové služby pro nasazení spuštěného a jeho zavedení postupně, pak filtrování provozu řešení známých nekompatibilita.</span><span class="sxs-lookup"><span data-stu-id="1ae49-245">We also touched on some powerful features of Vamp:  merging a new service variant to the running deployment and introducing it incrementally, then filtering traffic to resolve a known incompatibility.</span></span>


## <a name="next-steps"></a><span data-ttu-id="1ae49-246">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1ae49-246">Next steps</span></span>

* <span data-ttu-id="1ae49-247">Další informace o správě Vamp akce prostřednictvím [Vamp REST API](http://vamp.io/documentation/api/api-reference/).</span><span class="sxs-lookup"><span data-stu-id="1ae49-247">Learn about managing Vamp actions through the [Vamp REST API](http://vamp.io/documentation/api/api-reference/).</span></span>

* <span data-ttu-id="1ae49-248">Skripty pro automatizaci Vamp v Node.js sestavit a spustit je jako [Vamp pracovních](http://vamp.io/documentation/tutorials/create-a-workflow/).</span><span class="sxs-lookup"><span data-stu-id="1ae49-248">Build Vamp automation scripts in Node.js and run them as [Vamp workflows](http://vamp.io/documentation/tutorials/create-a-workflow/).</span></span>

* <span data-ttu-id="1ae49-249">Další informace najdete v [VAMP kurzy](http://vamp.io/documentation/tutorials/overview/).</span><span class="sxs-lookup"><span data-stu-id="1ae49-249">See additional [VAMP tutorials](http://vamp.io/documentation/tutorials/overview/).</span></span>

