---
title: verze aaaCanary s Vamp na clusteru Azure DC/OS | Microsoft Docs
description: "Jak toouse Vamp toocanary verze služby a použít filtrování v clusteru Azure Container Service DC/OS inteligentní provozu."
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
ms.openlocfilehash: e7b8658a161a7cddcf718e3e1c12a889a330d3d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="canary-release-microservices-with-vamp-on-an-azure-container-service-dcos-cluster"></a><span data-ttu-id="f672e-103">Lesknice verze mikroslužeb s Vamp v clusteru Azure Container Service DC/OS</span><span class="sxs-lookup"><span data-stu-id="f672e-103">Canary release microservices with Vamp on an Azure Container Service DC/OS cluster</span></span>

<span data-ttu-id="f672e-104">V tomto návodu budeme nastavit Vamp v Azure Container Service se cluster DC/OS.</span><span class="sxs-lookup"><span data-stu-id="f672e-104">In this walkthrough, we set up Vamp on Azure Container Service with a DC/OS cluster.</span></span> <span data-ttu-id="f672e-105">Jsme lesknice verze hello Vamp ukázku služby "Sávy" a potom vyřešte nekompatibility hello služby s Firefox použitím filtrování inteligentního přenosu.</span><span class="sxs-lookup"><span data-stu-id="f672e-105">We canary release hello Vamp demo service "sava", and then resolve an incompatibility of hello service with Firefox by applying smart traffic filtering.</span></span> 

> [!TIP] 
> <span data-ttu-id="f672e-106">V tomto návodu Vamp běží na clusteru DC/OS, ale můžete také použít Vamp s Kubernetes jako hello orchestrator.</span><span class="sxs-lookup"><span data-stu-id="f672e-106">In this walkthrough Vamp runs on a DC/OS cluster, but you can also use Vamp with Kubernetes as hello orchestrator.</span></span>
>

## <a name="about-canary-releases-and-vamp"></a><span data-ttu-id="f672e-107">O Kanárských uvolní a Vamp</span><span class="sxs-lookup"><span data-stu-id="f672e-107">About canary releases and Vamp</span></span>


<span data-ttu-id="f672e-108">[Lesknice uvolnění](https://martinfowler.com/bliki/CanaryRelease.html) je přijat inovativní organizacím, jako Netflix, Facebook či Spotify strategie inteligentní nasazení.</span><span class="sxs-lookup"><span data-stu-id="f672e-108">[Canary releasing](https://martinfowler.com/bliki/CanaryRelease.html) is a smart deployment strategy adopted by innovative organizations like Netflix, Facebook, and Spotify.</span></span> <span data-ttu-id="f672e-109">Je postup, který dává smysl, protože snižuje problémy, představuje bezpečnostní sítě a zvyšuje inovace.</span><span class="sxs-lookup"><span data-stu-id="f672e-109">It’s an approach that makes sense, because it reduces issues, introduces safety-nets, and increases innovation.</span></span> <span data-ttu-id="f672e-110">Proto proč všech společností, které nepoužívají ho?</span><span class="sxs-lookup"><span data-stu-id="f672e-110">So why aren’t all companies using it?</span></span> <span data-ttu-id="f672e-111">Rozšíření kanálu tooinclude CI/CD lesknice strategie přidá složitost a vyžaduje devops rozsáhlé znalosti a zkušenosti.</span><span class="sxs-lookup"><span data-stu-id="f672e-111">Extending a CI/CD pipeline tooinclude canary strategies adds complexity, and requires extensive devops knowledge and experience.</span></span> <span data-ttu-id="f672e-112">Předtím, než se i Začínáme je dostatek tooblock menší firem a podniků agentem.</span><span class="sxs-lookup"><span data-stu-id="f672e-112">That’s enough tooblock smaller companies and enterprises alike before they even get started.</span></span> 

<span data-ttu-id="f672e-113">[Vamp](http://vamp.io/) je tooease systém open source určené tento přechod a převeďte lesknice uvolnit scheduler kontejneru tooyour upřednostňovaný funkce.</span><span class="sxs-lookup"><span data-stu-id="f672e-113">[Vamp](http://vamp.io/) is an open-source system designed tooease this transition and bring canary releasing features tooyour preferred container scheduler.</span></span> <span data-ttu-id="f672e-114">Funkce lesknice na vamp překročí pohledu na základě procenta.</span><span class="sxs-lookup"><span data-stu-id="f672e-114">Vamp’s canary functionality goes beyond percentage-based rollouts.</span></span> <span data-ttu-id="f672e-115">Provoz můžete filtrovat a rozdělit na širokou škálu podmínky, například tootarget konkrétní uživatele, rozsahy IP adres nebo zařízení.</span><span class="sxs-lookup"><span data-stu-id="f672e-115">Traffic can be filtered and split on a wide range of conditions, for example tootarget specific users, IP-ranges, or devices.</span></span> <span data-ttu-id="f672e-116">Vamp sleduje a analyzuje metrik výkonu, povolení pro automatizaci na základě reálného dat.</span><span class="sxs-lookup"><span data-stu-id="f672e-116">Vamp tracks and analyzes performance metrics, allowing for automation based on real-world data.</span></span> <span data-ttu-id="f672e-117">Můžete nastavit automatické vrácení zpět na chyby, nebo škálování variant jednotlivých služeb na základě zatížení nebo latence.</span><span class="sxs-lookup"><span data-stu-id="f672e-117">You can set up automatic rollback on errors, or scale individual service variants based on load or latency.</span></span>

## <a name="set-up-azure-container-service-with-dcos"></a><span data-ttu-id="f672e-118">Nastavení Azure Container Service s DC/OS</span><span class="sxs-lookup"><span data-stu-id="f672e-118">Set up Azure Container Service with DC/OS</span></span>



1. <span data-ttu-id="f672e-119">[Nasadit cluster DC/OS](container-service-deployment.md) se jeden z nich a dva agenti výchozí velikost.</span><span class="sxs-lookup"><span data-stu-id="f672e-119">[Deploy a DC/OS cluster](container-service-deployment.md) with one master and two agents of default size.</span></span> 

2. <span data-ttu-id="f672e-120">[Vytvoření tunelu SSH](../container-service-connect.md) clusteru DC/OS toohello tooconnect.</span><span class="sxs-lookup"><span data-stu-id="f672e-120">[Create an SSH tunnel](../container-service-connect.md) tooconnect toohello DC/OS cluster.</span></span> <span data-ttu-id="f672e-121">Tento článek předpokládá, vytvoření tunelu toohello clusteru na místního portu 80.</span><span class="sxs-lookup"><span data-stu-id="f672e-121">This article assumes that you tunnel toohello cluster on local port 80.</span></span>


## <a name="set-up-vamp"></a><span data-ttu-id="f672e-122">Nastavit Vamp</span><span class="sxs-lookup"><span data-stu-id="f672e-122">Set up Vamp</span></span>

<span data-ttu-id="f672e-123">Teď, když máte spuštěného clusteru DC/OS, můžete nainstalovat Vamp z hello uživatelského rozhraní DC/OS (http://localhost:80).</span><span class="sxs-lookup"><span data-stu-id="f672e-123">Now that you have a running DC/OS cluster, you can install Vamp from hello DC/OS UI (http://localhost:80).</span></span> 

![Uživatelské rozhraní DC/OS](./media/container-service-dcos-vamp-canary-release/01_set_up_vamp.png)

<span data-ttu-id="f672e-125">Instalace probíhá ve dvou fázích:</span><span class="sxs-lookup"><span data-stu-id="f672e-125">Installation is done in two stages:</span></span>

1. <span data-ttu-id="f672e-126">**Nasazení Elasticsearch**.</span><span class="sxs-lookup"><span data-stu-id="f672e-126">**Deploy Elasticsearch**.</span></span>

2. <span data-ttu-id="f672e-127">Potom **nasazení Vamp** nainstalováním hello Vamp DC/OS universe balíčku.</span><span class="sxs-lookup"><span data-stu-id="f672e-127">Then **deploy Vamp** by installing hello Vamp DC/OS universe package.</span></span>

### <a name="deploy-elasticsearch"></a><span data-ttu-id="f672e-128">Nasazení Elasticsearch</span><span class="sxs-lookup"><span data-stu-id="f672e-128">Deploy Elasticsearch</span></span>

<span data-ttu-id="f672e-129">Vamp vyžaduje Elasticsearch metriky shromažďování a agregace.</span><span class="sxs-lookup"><span data-stu-id="f672e-129">Vamp requires Elasticsearch for metrics collection and aggregation.</span></span> <span data-ttu-id="f672e-130">Můžete použít hello [imagí Dockeru magneticio](https://hub.docker.com/r/magneticio/elastic/) toodeploy kompatibilní Vamp Elasticsearch zásobníku.</span><span class="sxs-lookup"><span data-stu-id="f672e-130">You can use hello [magneticio Docker images](https://hub.docker.com/r/magneticio/elastic/) toodeploy a compatible Vamp Elasticsearch stack.</span></span>

1. <span data-ttu-id="f672e-131">Hello uživatelského rozhraní DC/OS, přejděte v příliš**služby** a klikněte na tlačítko **nasadit službu**.</span><span class="sxs-lookup"><span data-stu-id="f672e-131">In hello DC/OS UI, go too**Services** and click **Deploy Service**.</span></span>

2. <span data-ttu-id="f672e-132">Vyberte **režim JSON** z hello **nasazení nové služby** automaticky otevírané okno.</span><span class="sxs-lookup"><span data-stu-id="f672e-132">Select **JSON mode** from hello **Deploy New Service** pop-up.</span></span>

  ![Vyberte režim JSON](./media/container-service-dcos-vamp-canary-release/02_deploy_service_json_mode.png)

3. <span data-ttu-id="f672e-134">Vložte následující JSON hello.</span><span class="sxs-lookup"><span data-stu-id="f672e-134">Paste in hello following JSON.</span></span> <span data-ttu-id="f672e-135">Tato konfigurace používá hello kontejner s 1 GB paměti RAM a kontrola základní stavu na portu Elasticsearch hello.</span><span class="sxs-lookup"><span data-stu-id="f672e-135">This configuration runs hello container with 1 GB of RAM and a basic health check on hello Elasticsearch port.</span></span>
  
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
  

3. <span data-ttu-id="f672e-136">Klikněte na tlačítko **nasazení**.</span><span class="sxs-lookup"><span data-stu-id="f672e-136">Click **Deploy**.</span></span>

  <span data-ttu-id="f672e-137">DC/OS nasadí kontejner Elasticsearch hello.</span><span class="sxs-lookup"><span data-stu-id="f672e-137">DC/OS deploys hello Elasticsearch container.</span></span> <span data-ttu-id="f672e-138">Průběh můžete sledovat na hello **služby** stránky.</span><span class="sxs-lookup"><span data-stu-id="f672e-138">You can track progress on hello **Services** page.</span></span>  

  ![nasazení e? Elasticsearch](./media/container-service-dcos-vamp-canary-release/03_deply_elasticsearch.png)

### <a name="deploy-vamp"></a><span data-ttu-id="f672e-140">Nasazení Vamp</span><span class="sxs-lookup"><span data-stu-id="f672e-140">Deploy Vamp</span></span>

<span data-ttu-id="f672e-141">Jakmile Elasticsearch sestavy jako **systémem**, můžete přidat balíček hello Vamp Universe DC/OS.</span><span class="sxs-lookup"><span data-stu-id="f672e-141">Once Elasticsearch reports as **Running**, you can add hello Vamp DC/OS Universe package.</span></span> 

1. <span data-ttu-id="f672e-142">Přejděte příliš**Universe** a vyhledejte **vamp**.</span><span class="sxs-lookup"><span data-stu-id="f672e-142">Go too**Universe** and search for **vamp**.</span></span> 
  <span data-ttu-id="f672e-143">![Vamp na DC/OS universe](./media/container-service-dcos-vamp-canary-release/04_universe_deploy_vamp.png)</span><span class="sxs-lookup"><span data-stu-id="f672e-143">![Vamp on DC/OS universe](./media/container-service-dcos-vamp-canary-release/04_universe_deploy_vamp.png)</span></span>

2. <span data-ttu-id="f672e-144">Klikněte na tlačítko **nainstalovat** další toohello vamp balíčku a vyberte **rozšířený instalace**.</span><span class="sxs-lookup"><span data-stu-id="f672e-144">Click **install** next toohello vamp package, and choose **Advanced Installation**.</span></span>

3. <span data-ttu-id="f672e-145">Posuňte se dolů a zadejte následující elasticsearch – adresa url hello: `http://elasticsearch.marathon.mesos:9200`.</span><span class="sxs-lookup"><span data-stu-id="f672e-145">Scroll down and enter hello following elasticsearch-url: `http://elasticsearch.marathon.mesos:9200`.</span></span> 

  ![Zadejte adresu URL Elasticsearch](./media/container-service-dcos-vamp-canary-release/05_universe_elasticsearch_url.png)

4. <span data-ttu-id="f672e-147">Klikněte na tlačítko **zkontrolovat a nainstalovat**, pak klikněte na tlačítko **nainstalovat** toostart hello nasazení.</span><span class="sxs-lookup"><span data-stu-id="f672e-147">Click **Review and Install**, then click **Install** toostart hello deployment.</span></span>  

  <span data-ttu-id="f672e-148">DC/OS nasadí všechny požadované součásti Vamp.</span><span class="sxs-lookup"><span data-stu-id="f672e-148">DC/OS deploys all required Vamp components.</span></span> <span data-ttu-id="f672e-149">Průběh můžete sledovat na hello **služby** stránky.</span><span class="sxs-lookup"><span data-stu-id="f672e-149">You can track progress on hello **Services** page.</span></span>
  
  ![Nasazení Vamp jako balíček universe](./media/container-service-dcos-vamp-canary-release/06_deploy_vamp.png)
  
5. <span data-ttu-id="f672e-151">Po dokončení nasazení se můžete dostat hello Vamp uživatelského rozhraní:</span><span class="sxs-lookup"><span data-stu-id="f672e-151">Once deployment has completed, you can access hello Vamp UI:</span></span>

  ![Služba vamp na DC/OS](./media/container-service-dcos-vamp-canary-release/07_deploy_vamp_complete.png)
  
  ![Vamp uživatelského rozhraní](./media/container-service-dcos-vamp-canary-release/08_vamp_ui.png)


## <a name="deploy-your-first-service"></a><span data-ttu-id="f672e-154">Nasazení vaší první službě</span><span class="sxs-lookup"><span data-stu-id="f672e-154">Deploy your first service</span></span>

<span data-ttu-id="f672e-155">Nyní tento Vamp je spuštěná, nasazení služby z plán, podle kterého.</span><span class="sxs-lookup"><span data-stu-id="f672e-155">Now that Vamp is up and running, deploy a service from a blueprint.</span></span> 

<span data-ttu-id="f672e-156">Ve své nejjednodušší podobě [Vamp plán, podle kterého](http://vamp.io/documentation/using-vamp/blueprints/) popisuje hello koncových bodů (brány), clustery a toodeploy služby.</span><span class="sxs-lookup"><span data-stu-id="f672e-156">In its simplest form, a [Vamp blueprint](http://vamp.io/documentation/using-vamp/blueprints/) describes hello endpoints (gateways), clusters, and services toodeploy.</span></span> <span data-ttu-id="f672e-157">Vamp používá clustery toogroup různé varianty hello stejné služby do logických skupin pro lesknice uvolněním nebo A / B testování.</span><span class="sxs-lookup"><span data-stu-id="f672e-157">Vamp uses clusters toogroup different variants of hello same service into logical groups for canary releasing or A/B testing.</span></span>  

<span data-ttu-id="f672e-158">Tento scénář používá ukázkovou aplikaci monolitický názvem [ **Sávy**](https://github.com/magneticio/sava), který je ve verzi 1.0.</span><span class="sxs-lookup"><span data-stu-id="f672e-158">This scenario uses a sample monolithic application called [**sava**](https://github.com/magneticio/sava), which is at version 1.0.</span></span> <span data-ttu-id="f672e-159">monolitu Hello je součástí Docker kontejneru, který je v úložiště Docker Hub v rámci magneticio/sava:1.0.0.</span><span class="sxs-lookup"><span data-stu-id="f672e-159">hello monolith is packaged in a Docker container, which is in Docker Hub under magneticio/sava:1.0.0.</span></span> <span data-ttu-id="f672e-160">aplikace Hello za normálních okolností běží na portu 8080, ale chcete ho v tomto případě portu 9050 tooexpose.</span><span class="sxs-lookup"><span data-stu-id="f672e-160">hello app normally runs on port 8080, but you want tooexpose it under port 9050 in this case.</span></span> <span data-ttu-id="f672e-161">Nasazení aplikace hello prostřednictvím Vamp používá jednoduchý plán, podle kterého.</span><span class="sxs-lookup"><span data-stu-id="f672e-161">Deploy hello app through Vamp using a simple blueprint.</span></span>

1. <span data-ttu-id="f672e-162">Přejděte příliš**nasazení**.</span><span class="sxs-lookup"><span data-stu-id="f672e-162">Go too**Deployments**.</span></span>

2. <span data-ttu-id="f672e-163">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="f672e-163">Click **Add**.</span></span>

3. <span data-ttu-id="f672e-164">Vložte následující hello plán, podle kterého YAML.</span><span class="sxs-lookup"><span data-stu-id="f672e-164">Paste in hello following blueprint YAML.</span></span> <span data-ttu-id="f672e-165">Tento plán, podle kterého obsahuje jeden cluster s variant pouze jedna služba, která nám změnit později:</span><span class="sxs-lookup"><span data-stu-id="f672e-165">This blueprint contains one cluster with only one service variant, which we change in a later step:</span></span>

  ```YAML
  name: sava                        # deployment name
  gateways:
    9050: sava_cluster/webport      # stable endpoint
  clusters:
    sava_cluster:               # cluster toocreate
     services:
        -
          breed:
            name: sava:1.0.0        # service variant name
            deployable: magneticio/sava:1.0.0
            ports:
              webport: 8080/http # cluster endpoint, used for canary releasing
  ```

4. <span data-ttu-id="f672e-166">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="f672e-166">Click **Save**.</span></span> <span data-ttu-id="f672e-167">Vamp zahájí hello nasazení.</span><span class="sxs-lookup"><span data-stu-id="f672e-167">Vamp initiates hello deployment.</span></span>

<span data-ttu-id="f672e-168">nasazení Hello je uvedená na hello **nasazení** stránky.</span><span class="sxs-lookup"><span data-stu-id="f672e-168">hello deployment is listed on hello **Deployments** page.</span></span> <span data-ttu-id="f672e-169">Klikněte na tlačítko hello nasazení toomonitor jeho stav.</span><span class="sxs-lookup"><span data-stu-id="f672e-169">Click hello deployment toomonitor its status.</span></span>

![Uživatelské rozhraní – nasazení Sávy vamp](./media/container-service-dcos-vamp-canary-release/09_sava100.png)

![Služba Sávy v Vamp uživatelského rozhraní](./media/container-service-dcos-vamp-canary-release/09a_sava100.png)

<span data-ttu-id="f672e-172">Vytváří dvě brány, které jsou uvedeny na hello **brány** stránky:</span><span class="sxs-lookup"><span data-stu-id="f672e-172">Two gateways are created, which are listed on hello **Gateways** page:</span></span>

* <span data-ttu-id="f672e-173">Dobrý den tooaccess stabilní koncový bod službou (port 9050)</span><span class="sxs-lookup"><span data-stu-id="f672e-173">a stable endpoint tooaccess hello running service (port 9050)</span></span> 
* <span data-ttu-id="f672e-174">spravované Vamp interní brány (Další informace o této brány se později).</span><span class="sxs-lookup"><span data-stu-id="f672e-174">a Vamp-managed internal gateway (more on this gateway later).</span></span> 

![Vamp uživatelské rozhraní – Sávy brány](./media/container-service-dcos-vamp-canary-release/10_vamp_sava_gateways.png)

<span data-ttu-id="f672e-176">službu Sávy Hello má nyní nasadit, ale protože tooforward provoz tooit není známo ještě hello nástroj pro vyrovnávání zatížení Azure ho nelze přístup externě.</span><span class="sxs-lookup"><span data-stu-id="f672e-176">hello sava service has now deployed, but you can’t access it externally because hello Azure Load Balancer doesn’t know tooforward traffic tooit yet.</span></span> <span data-ttu-id="f672e-177">tooaccess hello služba hello aktualizace konfigurace sítí Azure.</span><span class="sxs-lookup"><span data-stu-id="f672e-177">tooaccess hello service, update hello Azure networking configuration.</span></span>


## <a name="update-hello-azure-network-configuration"></a><span data-ttu-id="f672e-178">Aktualizace konfigurace hello síť Azure</span><span class="sxs-lookup"><span data-stu-id="f672e-178">Update hello Azure network configuration</span></span>

<span data-ttu-id="f672e-179">Služba Sávy hello vamp nasazené na uzlech agenta DC/OS hello, vystavení stabilní koncový bod na port 9050.</span><span class="sxs-lookup"><span data-stu-id="f672e-179">Vamp deployed hello sava service on hello DC/OS agent nodes, exposing a stable endpoint at port 9050.</span></span> <span data-ttu-id="f672e-180">tooaccess hello služby z clusteru DC/OS mimo hello, proveďte následující změny konfigurace Azure sítě toohello ve vašem nasazení clusteru hello:</span><span class="sxs-lookup"><span data-stu-id="f672e-180">tooaccess hello service from outside hello DC/OS cluster, make hello following changes toohello Azure network configuration in your cluster deployment:</span></span> 

1. <span data-ttu-id="f672e-181">**Konfigurace hello nástroj pro vyrovnávání zatížení Azure** pro agenty hello (hello prostředek s názvem **orchestrátoru DC/OS agenta lb xxxx**) s test stavu a přenosem tooforward pravidlo na portu 9050 toohello Sávy instancí.</span><span class="sxs-lookup"><span data-stu-id="f672e-181">**Configure hello Azure Load Balancer** for hello agents (hello resource named **dcos-agent-lb-xxxx**) with a health probe and a rule tooforward traffic on port 9050 toohello sava instances.</span></span> 

2. <span data-ttu-id="f672e-182">**Skupina zabezpečení sítě hello aktualizace** pro veřejné agenty hello (hello prostředek s názvem **XXXX-agent veřejný nsg-XXXX**) tooallow přenosy na portu 9050.</span><span class="sxs-lookup"><span data-stu-id="f672e-182">**Update hello network security group** for hello public agents (hello resource named **XXXX-agent-public-nsg-XXXX**) tooallow traffic on port 9050.</span></span>

<span data-ttu-id="f672e-183">Pro podrobné kroky toocomplete hello tyto úlohy pomocí portálu Azure najdete [povolit aplikaci Azure Container Service tooan veřejný přístup](container-service-enable-public-access.md).</span><span class="sxs-lookup"><span data-stu-id="f672e-183">For detailed steps toocomplete these tasks using hello Azure portal, see [Enable public access tooan Azure Container Service application](container-service-enable-public-access.md).</span></span> <span data-ttu-id="f672e-184">Zadejte port 9050 pro všechna nastavení portu.</span><span class="sxs-lookup"><span data-stu-id="f672e-184">Specify port 9050 for all port settings.</span></span>


<span data-ttu-id="f672e-185">Po všechno, co byla vytvořena, přejděte toohello **přehled** okno služby Vyrovnávání zatížení agenta DC/OS hello (hello prostředek s názvem **orchestrátoru DC/OS agenta lb xxxx**).</span><span class="sxs-lookup"><span data-stu-id="f672e-185">Once everything has been created, go toohello **Overview** blade of hello DC/OS agent load balancer (hello resource named **dcos-agent-lb-xxxx**).</span></span> <span data-ttu-id="f672e-186">Najde hello **veřejnou IP adresu**a použití hello adresu tooaccess Sávy na portu 9050.</span><span class="sxs-lookup"><span data-stu-id="f672e-186">Find hello **Public IP address**, and use hello address tooaccess sava at port 9050.</span></span>

![Portál Azure – get veřejnou IP adresu](./media/container-service-dcos-vamp-canary-release/18_public_ip_address.png)

![Sávy](./media/container-service-dcos-vamp-canary-release/19_sava100.png)


## <a name="run-a-canary-release"></a><span data-ttu-id="f672e-189">Spustit lesknice verze</span><span class="sxs-lookup"><span data-stu-id="f672e-189">Run a canary release</span></span>

<span data-ttu-id="f672e-190">Předpokládejme, že máte novou verzi této aplikace, které chcete toocanary verze do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="f672e-190">Suppose you have a new version of this application that you want toocanary release into production.</span></span> <span data-ttu-id="f672e-191">Ji kontejnerizované jako magneticio/sava:1.1.0 a jsou připravené toogo.</span><span class="sxs-lookup"><span data-stu-id="f672e-191">You have it containerized as magneticio/sava:1.1.0 and are ready toogo.</span></span> <span data-ttu-id="f672e-192">Vamp umožňuje snadno přidat nové služby toohello běží nasazení.</span><span class="sxs-lookup"><span data-stu-id="f672e-192">Vamp lets you easily add new services toohello running deployment.</span></span> <span data-ttu-id="f672e-193">Tyto služby "sloučené" jsou nasadit souběžně s hello stávající služby v clusteru hello a přiřadili váhu % 0.</span><span class="sxs-lookup"><span data-stu-id="f672e-193">These "merged" services are deployed alongside hello existing services in hello cluster, and assigned a weight of 0%.</span></span> <span data-ttu-id="f672e-194">Žádný provoz je služba směrované tooa nově sloučit, dokud upravit distribuce přenosů hello.</span><span class="sxs-lookup"><span data-stu-id="f672e-194">No traffic is routed tooa newly merged service until you adjust hello traffic distribution.</span></span> <span data-ttu-id="f672e-195">jezdec váhy Hello v hello Vamp uživatelského rozhraní poskytuje úplnou kontrolu nad hello distribuce, aby vám umožnil přírůstkové úpravy (lesknice vydání) nebo rychlých vrácení zpět.</span><span class="sxs-lookup"><span data-stu-id="f672e-195">hello weight slider in hello Vamp UI gives you complete control over hello distribution, allowing for incremental adjustments (canary release) or an instant rollback.</span></span>

### <a name="merge-a-new-service-variant"></a><span data-ttu-id="f672e-196">Sloučení nové služby variant</span><span class="sxs-lookup"><span data-stu-id="f672e-196">Merge a new service variant</span></span>

<span data-ttu-id="f672e-197">toomerge hello nové Sávy 1.1 služby s hello běží nasazení:</span><span class="sxs-lookup"><span data-stu-id="f672e-197">toomerge hello new sava 1.1 service with hello running deployment:</span></span>

1. <span data-ttu-id="f672e-198">V hello Vamp uživatelského rozhraní, klikněte na **plány vyfotografovat**.</span><span class="sxs-lookup"><span data-stu-id="f672e-198">In hello Vamp UI, click **Blueprints**.</span></span>

2. <span data-ttu-id="f672e-199">Klikněte na tlačítko **přidat** a vložte následující hello plán, podle kterého YAML: Tento plán, podle kterého popisuje nové toodeploy typu variant (Sávy: 1.1.0) služby v rámci stávajícího clusteru hello (sava_cluster).</span><span class="sxs-lookup"><span data-stu-id="f672e-199">Click **Add** and paste in hello following blueprint YAML: This blueprint describes a new service variant (sava:1.1.0) toodeploy within hello existing cluster (sava_cluster).</span></span>

  ```YAML
  name: sava:1.1.0      # blueprint name
  clusters:
    sava_cluster:       # cluster tooupdate
      services:
        -
          breed:
            name: sava:1.1.0    # service variant name
            deployable: magneticio/sava:1.1.0    
            ports:
              webport: 8080/http # cluster endpoint tooupdate
  ```
  
3. <span data-ttu-id="f672e-200">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="f672e-200">Click **Save**.</span></span> <span data-ttu-id="f672e-201">Hello plán, podle kterého je uložena a uvedené na hello **plány vyfotografovat** stránky.</span><span class="sxs-lookup"><span data-stu-id="f672e-201">hello blueprint is stored and listed on hello **Blueprints** page.</span></span>

4. <span data-ttu-id="f672e-202">Nabídka Akce otevřete hello na plán, podle kterého Sávy: 1.1 hello a klikněte na **sloučí se**.</span><span class="sxs-lookup"><span data-stu-id="f672e-202">Open hello action menu on hello sava:1.1 blueprint and click **Merge to**.</span></span>

  ![Vamp uživatelské rozhraní – plány](./media/container-service-dcos-vamp-canary-release/20_sava110_mergeto.png)

5. <span data-ttu-id="f672e-204">Vyberte hello **Sávy** nasazení a klikněte na tlačítko **sloučení**.</span><span class="sxs-lookup"><span data-stu-id="f672e-204">Select hello **sava** deployment and click **Merge**.</span></span>

  ![Vamp uživatelského rozhraní – sloučení plán, podle kterého toodeployment](./media/container-service-dcos-vamp-canary-release/21_sava110_merge.png)

<span data-ttu-id="f672e-206">Vamp nasadí hello nové Sávy: 1.1.0 služby typu variant popsané v hello plán, podle kterého spolu s Sávy: 1.0.0 v hello **sava_cluster** z hello běží nasazení.</span><span class="sxs-lookup"><span data-stu-id="f672e-206">Vamp deploys hello new sava:1.1.0 service variant described in hello blueprint alongside sava:1.0.0 in hello **sava_cluster** of hello running deployment.</span></span> 

![Vamp uživatelské rozhraní – aktualizované Sávy nasazení](./media/container-service-dcos-vamp-canary-release/22_sava_cluster.png)

<span data-ttu-id="f672e-208">Hello **Sávy/sava_cluster/webport** brány (koncový bod clusteru hello) je rovněž aktualizováno, přidání trasy toohello nově nasazené Sávy: 1.1.0.</span><span class="sxs-lookup"><span data-stu-id="f672e-208">hello **sava/sava_cluster/webport** gateway (hello cluster endpoint) is also updated, adding a route toohello newly deployed sava:1.1.0.</span></span> <span data-ttu-id="f672e-209">V tomto okamžiku žádný provoz se směruje zde (hello **VÁHY** nastavena too0 %).</span><span class="sxs-lookup"><span data-stu-id="f672e-209">At this point, no traffic is routed here (hello **WEIGHT** is set too0%).</span></span>

![Vamp uživatelské rozhraní – clusteru brány](./media/container-service-dcos-vamp-canary-release/23_sava_cluster_webport.png)

### <a name="canary-release"></a><span data-ttu-id="f672e-211">Lesknice verze</span><span class="sxs-lookup"><span data-stu-id="f672e-211">Canary release</span></span>

<span data-ttu-id="f672e-212">V obou verzích Sávy nasazené v hello stejný cluster, upravte hello distribuce přenosů mezi nimi přesunutím hello **VÁHY** posuvníku.</span><span class="sxs-lookup"><span data-stu-id="f672e-212">With both versions of sava deployed in hello same cluster, adjust hello distribution of traffic between them by moving hello **WEIGHT** slider.</span></span>

1. <span data-ttu-id="f672e-213">Klikněte na tlačítko ![Vamp uživatelské rozhraní – upravit](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) další příliš**VÁHY**.</span><span class="sxs-lookup"><span data-stu-id="f672e-213">Click ![Vamp UI - edit](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) next too**WEIGHT**.</span></span>

2. <span data-ttu-id="f672e-214">Nastavit too50%/50% distribuce váhy hello a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="f672e-214">Set hello weight distribution too50%/50% and click **Save**.</span></span>

  ![Vamp uživatelské rozhraní – brána váhy posuvníku](./media/container-service-dcos-vamp-canary-release/24_sava_cluster_webport_weight.png)

3. <span data-ttu-id="f672e-216">Přejděte zpět tooyour prohlížeče a aktualizujte stránku hello Sávy několik vícekrát.</span><span class="sxs-lookup"><span data-stu-id="f672e-216">Go back tooyour browser and refresh hello sava page a few more times.</span></span> <span data-ttu-id="f672e-217">Hello Sávy nyní aplikaci přepíná mezi Sávy: 1.0 stránky a stránky Sávy: 1.1.</span><span class="sxs-lookup"><span data-stu-id="f672e-217">hello sava application now switches between a sava:1.0 page and a sava:1.1 page.</span></span>

  ![střídání sava1.0 a sava1.1 služby](./media/container-service-dcos-vamp-canary-release/25_sava_100_101.png)


  > [!NOTE]
  > <span data-ttu-id="f672e-219">Tato alternace stránku hello nejvhodnější hello "Incognito" nebo "Anonymní" režim prohlížeče z důvodu hello ukládání do mezipaměti statické prostředky.</span><span class="sxs-lookup"><span data-stu-id="f672e-219">This alternation of hello page works best with hello "Incognito" or “Anonymous” mode of your browser because of hello caching of static assets.</span></span>
  >

### <a name="filter-traffic"></a><span data-ttu-id="f672e-220">Filtrování provozu</span><span class="sxs-lookup"><span data-stu-id="f672e-220">Filter traffic</span></span>

<span data-ttu-id="f672e-221">Po nasazení Předpokládejme, že jste zjišťuje nekompatibilitu v Sávy: 1.1.0, že příčiny zobrazit problémy v prohlížečích Firefox.</span><span class="sxs-lookup"><span data-stu-id="f672e-221">Suppose after deployment that you discovered an incompatibility in sava:1.1.0 that causes display problems in Firefox browsers.</span></span> <span data-ttu-id="f672e-222">Můžete nastavit Vamp toofilter příchozí provoz a přímé, že všichni uživatelé Firefox zpět toohello známé stabilní Sávy: 1.0.0.</span><span class="sxs-lookup"><span data-stu-id="f672e-222">You can set Vamp toofilter incoming traffic and direct all Firefox users back toohello known stable sava:1.0.0.</span></span> <span data-ttu-id="f672e-223">Tento filtr okamžitě vyřeší hello přerušení pro uživatele, Firefox, zatímco ostatní uživatelé stále tooenjoy hello výhod hello vylepšené Sávy: 1.1.0.</span><span class="sxs-lookup"><span data-stu-id="f672e-223">This filter instantly resolves hello disruption for Firefox users, while everybody else continues tooenjoy hello benefits of hello improved sava:1.1.0.</span></span>

<span data-ttu-id="f672e-224">Vamp používá **podmínky** toofilter provoz mezi trasy v bránu.</span><span class="sxs-lookup"><span data-stu-id="f672e-224">Vamp uses **conditions** toofilter traffic between routes in a gateway.</span></span> <span data-ttu-id="f672e-225">Provoz je nejdřív filtrovaný a přesměruje podle toohello podmínky použijí tooeach trasy.</span><span class="sxs-lookup"><span data-stu-id="f672e-225">Traffic is first filtered and directed according toohello conditions applied tooeach route.</span></span> <span data-ttu-id="f672e-226">Veškerý zbývající provoz je distribuován podle nastavení váhy toohello brány.</span><span class="sxs-lookup"><span data-stu-id="f672e-226">All remaining traffic is distributed according toohello gateway weight setting.</span></span>

<span data-ttu-id="f672e-227">Můžete vytvořit podmínku toofilter všichni uživatelé Firefox a směrovat je starý Sávy: 1.0.0 toohello:</span><span class="sxs-lookup"><span data-stu-id="f672e-227">You can create a condition toofilter all Firefox users and direct them toohello old sava:1.0.0:</span></span>

1. <span data-ttu-id="f672e-228">Na hello Sávy/sava_cluster/webport **brány** klikněte na tlačítko ![Vamp uživatelské rozhraní – upravit](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) tooadd **PODMÍNKU** sava/sava_cluster/sava:1.0.0/webport toohello trasy.</span><span class="sxs-lookup"><span data-stu-id="f672e-228">On hello sava/sava_cluster/webport **Gateways** page, click ![Vamp UI - edit](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) tooadd a **CONDITION** toohello route sava/sava_cluster/sava:1.0.0/webport.</span></span> 

2. <span data-ttu-id="f672e-229">Zadejte podmínky hello **uživatelský agent == Firefox** a klikněte na tlačítko ![Vamp uživatelské rozhraní – Uložit](./media/container-service-dcos-vamp-canary-release/vamp_ui_save.png).</span><span class="sxs-lookup"><span data-stu-id="f672e-229">Enter hello condition **user-agent == Firefox** and click ![Vamp UI - save](./media/container-service-dcos-vamp-canary-release/vamp_ui_save.png).</span></span>

  <span data-ttu-id="f672e-230">Vamp přidá hello podmínky se výchozí sílu % 0.</span><span class="sxs-lookup"><span data-stu-id="f672e-230">Vamp adds hello condition with a default strength of 0%.</span></span> <span data-ttu-id="f672e-231">filtrování provozu toostart, musíte tooadjust hello podmínku sílu.</span><span class="sxs-lookup"><span data-stu-id="f672e-231">toostart filtering traffic, you need tooadjust hello condition strength.</span></span>

3. <span data-ttu-id="f672e-232">Klikněte na tlačítko ![Vamp uživatelské rozhraní – upravit](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) toochange hello **SÍLU** použít toohello podmínku.</span><span class="sxs-lookup"><span data-stu-id="f672e-232">Click ![Vamp UI - edit](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) toochange hello **STRENGTH** applied toohello condition.</span></span>
 
4. <span data-ttu-id="f672e-233">Sada hello **SÍLU** too100 % a klikněte na tlačítko ![Vamp uživatelské rozhraní – Uložit](./media/container-service-dcos-vamp-canary-release/vamp_ui_save.png) toosave.</span><span class="sxs-lookup"><span data-stu-id="f672e-233">Set hello **STRENGTH** too100% and click ![Vamp UI - save](./media/container-service-dcos-vamp-canary-release/vamp_ui_save.png) toosave.</span></span>

  <span data-ttu-id="f672e-234">Vamp teď odešle veškerý provoz odpovídající toosava:1.0.0 hello podmínku (všichni uživatelé Firefox).</span><span class="sxs-lookup"><span data-stu-id="f672e-234">Vamp now sends all traffic matching hello condition (all Firefox users) toosava:1.0.0.</span></span>

  ![Vamp uživatelského rozhraní – použít podmínku toogateway](./media/container-service-dcos-vamp-canary-release/26_apply_condition.png)

5. <span data-ttu-id="f672e-236">Nakonec upravte hello brány váhy toosend všechny zbývající provoz (všechny uživatele bez Firefox) toohello nové Sávy: 1.1.0.</span><span class="sxs-lookup"><span data-stu-id="f672e-236">Finally, adjust hello gateway weight toosend all remaining traffic (all non-Firefox users) toohello new sava:1.1.0.</span></span> <span data-ttu-id="f672e-237">Klikněte na tlačítko ![Vamp uživatelské rozhraní – upravit](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) další příliš**VÁHY** a nastavte distribuce váhy hello tak, aby 100 % sava/sava_cluster/sava:1.1.0/webport směrovanou toohello trasy.</span><span class="sxs-lookup"><span data-stu-id="f672e-237">Click ![Vamp UI - edit](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) next too**WEIGHT** and set hello weight distribution so 100% is directed toohello route sava/sava_cluster/sava:1.1.0/webport.</span></span>

  <span data-ttu-id="f672e-238">Veškerý provoz nejsou filtrovány podle hello podmínka je nyní směrovanou toohello nové Sávy: 1.1.0.</span><span class="sxs-lookup"><span data-stu-id="f672e-238">All traffic not filtered by hello condition is now directed toohello new sava:1.1.0.</span></span>

6. <span data-ttu-id="f672e-239">Filtr hello toosee v akci, otevřete dva různé prohlížeče (jeden Firefox a jeden jiný prohlížeč) a přístup k službě Sávy hello z obou.</span><span class="sxs-lookup"><span data-stu-id="f672e-239">toosee hello filter in action, open two different browsers (one Firefox and one other browser) and access hello sava service from both.</span></span> <span data-ttu-id="f672e-240">Všechny požadavky Firefox odešlou toosava:1.0.0, zatímco všech dalších prohlížečích jsou řízené toosava:1.1.0.</span><span class="sxs-lookup"><span data-stu-id="f672e-240">All Firefox requests are sent toosava:1.0.0, while all other browsers are directed toosava:1.1.0.</span></span>

  ![Vamp uživatelské rozhraní – filtrovat provoz](./media/container-service-dcos-vamp-canary-release/27_filter_traffic.png)

## <a name="summing-up"></a><span data-ttu-id="f672e-242">Shrnutí</span><span class="sxs-lookup"><span data-stu-id="f672e-242">Summing up</span></span>

<span data-ttu-id="f672e-243">Tento článek byl rychlý úvod tooVamp na cluster DC/OS.</span><span class="sxs-lookup"><span data-stu-id="f672e-243">This article was a quick introduction tooVamp on a DC/OS cluster.</span></span> <span data-ttu-id="f672e-244">Pro začátek jste získali Vamp a spuštěná na vaše Azure Container Service DC/OS clusteru, nasazení služby s Vamp plán, podle kterého a přístup na adrese koncový bod vystavený hello (brány).</span><span class="sxs-lookup"><span data-stu-id="f672e-244">For starters, you got Vamp up and running on your Azure Container Service DC/OS cluster, deployed a service with a Vamp blueprint, and accessed it at hello exposed endpoint (gateway).</span></span>

<span data-ttu-id="f672e-245">Můžeme také dotýkal na některé výkonné funkce Vamp: sloučení nové služby variant toohello běží nasazení a jeho zavedení postupně, a filtrování provozu tooresolve známé nekompatibilita.</span><span class="sxs-lookup"><span data-stu-id="f672e-245">We also touched on some powerful features of Vamp:  merging a new service variant toohello running deployment and introducing it incrementally, then filtering traffic tooresolve a known incompatibility.</span></span>


## <a name="next-steps"></a><span data-ttu-id="f672e-246">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f672e-246">Next steps</span></span>

* <span data-ttu-id="f672e-247">Další informace o správě Vamp akce prostřednictvím hello [Vamp REST API](http://vamp.io/documentation/api/api-reference/).</span><span class="sxs-lookup"><span data-stu-id="f672e-247">Learn about managing Vamp actions through hello [Vamp REST API](http://vamp.io/documentation/api/api-reference/).</span></span>

* <span data-ttu-id="f672e-248">Skripty pro automatizaci Vamp v Node.js sestavit a spustit je jako [Vamp pracovních](http://vamp.io/documentation/tutorials/create-a-workflow/).</span><span class="sxs-lookup"><span data-stu-id="f672e-248">Build Vamp automation scripts in Node.js and run them as [Vamp workflows](http://vamp.io/documentation/tutorials/create-a-workflow/).</span></span>

* <span data-ttu-id="f672e-249">Další informace najdete v [VAMP kurzy](http://vamp.io/documentation/tutorials/overview/).</span><span class="sxs-lookup"><span data-stu-id="f672e-249">See additional [VAMP tutorials](http://vamp.io/documentation/tutorials/overview/).</span></span>

