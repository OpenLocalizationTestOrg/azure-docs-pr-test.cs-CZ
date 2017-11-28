---
title: "aaaLoad vyvážit Kubernetes kontejnerů v Azure | Microsoft Docs"
description: "Připojte externě a vyrovnávání zatížení v rámci několika kontejnerů v clusteru s podporou Kubernetes v Azure Container Service."
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: Kubernetes kontejnery, Micro-services, Azure
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/17/2017
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: 8073c8d3a015a53a532c326749571cb2582e1bac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="load-balance-containers-in-a-kubernetes-cluster-in-azure-container-service"></a><span data-ttu-id="e6c8b-104">Kontejnery Vyrovnávání zatížení v clusteru s podporou Kubernetes v Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="e6c8b-104">Load balance containers in a Kubernetes cluster in Azure Container Service</span></span> 
<span data-ttu-id="e6c8b-105">Tento článek představuje Vyrovnávání zatížení v clusteru s podporou Kubernetes v Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="e6c8b-105">This article introduces load balancing in a Kubernetes cluster in Azure Container Service.</span></span> <span data-ttu-id="e6c8b-106">Vyrovnávání zatížení poskytuje IP adresu externě dostupný pro službu hello a distribuuje síťový provoz mezi hello pracovními stanicemi soustředěnými kolem spuštěna v agentovi virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="e6c8b-106">Load balancing provides an externally accessible IP address for hello service and distributes network traffic among hello pods running in agent VMs.</span></span>

<span data-ttu-id="e6c8b-107">Můžete nastavit toouse služby Kubernetes [nástroj pro vyrovnávání zatížení Azure](../../load-balancer/load-balancer-overview.md) toomanage externí zatížení sítě (TCP).</span><span class="sxs-lookup"><span data-stu-id="e6c8b-107">You can set up a Kubernetes service toouse [Azure Load Balancer](../../load-balancer/load-balancer-overview.md) toomanage external network (TCP) traffic.</span></span> <span data-ttu-id="e6c8b-108">Další konfiguraci je možné, směrování provozu protokolu HTTP nebo HTTPS nebo pokročilejší scénáře a vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="e6c8b-108">With additional configuration, load balancing and routing of HTTP or HTTPS traffic or more advanced scenarios are possible.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e6c8b-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e6c8b-109">Prerequisites</span></span>
* <span data-ttu-id="e6c8b-110">[Nasazení clusteru Kubernetes](container-service-kubernetes-walkthrough.md) v Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="e6c8b-110">[Deploy a Kubernetes cluster](container-service-kubernetes-walkthrough.md) in Azure Container Service</span></span>
* <span data-ttu-id="e6c8b-111">[Připojení vašeho klienta](../container-service-connect.md) tooyour clusteru</span><span class="sxs-lookup"><span data-stu-id="e6c8b-111">[Connect your client](../container-service-connect.md) tooyour cluster</span></span>

## <a name="azure-load-balancer"></a><span data-ttu-id="e6c8b-112">Nástroje pro vyrovnávání zatížení Azure</span><span class="sxs-lookup"><span data-stu-id="e6c8b-112">Azure load balancer</span></span>

<span data-ttu-id="e6c8b-113">Ve výchozím nastavení clusteru s podporou Kubernetes nasazené v Azure Container Service zahrnuje pro vyrovnávání zatížení Azure internetového pro agenta hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="e6c8b-113">By default, a Kubernetes cluster deployed in Azure Container Service includes an Internet-facing Azure load balancer for hello agent VMs.</span></span> <span data-ttu-id="e6c8b-114">(Prostředek pro vyrovnávání zatížení samostatné je nakonfigurován pro hlavní hello virtuálních počítačů). Nástroje pro vyrovnávání zatížení Azure je nástroj pro vyrovnávání zatížení vrstvy 4.</span><span class="sxs-lookup"><span data-stu-id="e6c8b-114">(A separate load balancer resource is configured for hello master VMs.) Azure load balancer is a Layer 4 load balancer.</span></span> <span data-ttu-id="e6c8b-115">Nástroj pro vyrovnávání zatížení hello v současné době podporuje pouze provoz protokolu TCP v Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="e6c8b-115">Currently, hello load balancer only supports TCP traffic in Kubernetes.</span></span>

<span data-ttu-id="e6c8b-116">Při vytváření služby Kubernetes, můžete nakonfigurovat automaticky hello Azure služby Vyrovnávání zatížení při tooallow přístup toohello.</span><span class="sxs-lookup"><span data-stu-id="e6c8b-116">When creating a Kubernetes service, you can automatically configure hello Azure load balancer tooallow access toohello service.</span></span> <span data-ttu-id="e6c8b-117">tooconfigure hello Vyrovnávání zatížení služby hello sadu `type` příliš`LoadBalancer`.</span><span class="sxs-lookup"><span data-stu-id="e6c8b-117">tooconfigure hello load balancer, set hello service `type` too`LoadBalancer`.</span></span> <span data-ttu-id="e6c8b-118">Nástroj pro vyrovnávání zatížení Hello vytvoří pravidlo toomap veřejnou IP adresu a číslo portu příchozí provoz toohello privátní IP adresy služby a čísla portů hello pracovními stanicemi soustředěnými kolem v agentovi virtuální počítače (a naopak přenosům odpověď).</span><span class="sxs-lookup"><span data-stu-id="e6c8b-118">hello load balancer creates a rule toomap a public IP address and port number of incoming service traffic toohello private IP addresses and port numbers of hello pods in agent VMs (and vice versa for response traffic).</span></span> 

 <span data-ttu-id="e6c8b-119">Následují dva příklady znázorňující, jak tooset hello Kubernetes služby `type` příliš`LoadBalancer`.</span><span class="sxs-lookup"><span data-stu-id="e6c8b-119">Following are two examples showing how tooset hello Kubernetes service `type` too`LoadBalancer`.</span></span> <span data-ttu-id="e6c8b-120">(Po snažíme hello příklady odstraňte hello nasazení je již nepotřebujete.)</span><span class="sxs-lookup"><span data-stu-id="e6c8b-120">(After trying hello examples, delete hello deployments if you no longer need them.)</span></span>

### <a name="example-use-hello-kubectl-expose-command"></a><span data-ttu-id="e6c8b-121">Příklad: Použití hello `kubectl expose` příkaz</span><span class="sxs-lookup"><span data-stu-id="e6c8b-121">Example: Use hello `kubectl expose` command</span></span> 
<span data-ttu-id="e6c8b-122">Hello [Kubernetes návod](container-service-kubernetes-walkthrough.md) obsahuje příklad tooexpose služby s hello `kubectl expose` příkazu a jeho `--type=LoadBalancer` příznak.</span><span class="sxs-lookup"><span data-stu-id="e6c8b-122">hello [Kubernetes walkthrough](container-service-kubernetes-walkthrough.md) includes an example of how tooexpose a service with hello `kubectl expose` command and its `--type=LoadBalancer` flag.</span></span> <span data-ttu-id="e6c8b-123">Zde jsou kroky hello:</span><span class="sxs-lookup"><span data-stu-id="e6c8b-123">Here are hello steps :</span></span>

1. <span data-ttu-id="e6c8b-124">Spuštění nového nasazení kontejneru.</span><span class="sxs-lookup"><span data-stu-id="e6c8b-124">Start a new container deployment.</span></span> <span data-ttu-id="e6c8b-125">Například hello následující příkaz spustí názvem nového nasazení `mynginx`.</span><span class="sxs-lookup"><span data-stu-id="e6c8b-125">For example, hello following command starts a new deployment called `mynginx`.</span></span> <span data-ttu-id="e6c8b-126">Hello nasazení se skládá z tři kontejnery na základě bitové kopie Docker hello hello Nginx webovém serveru.</span><span class="sxs-lookup"><span data-stu-id="e6c8b-126">hello deployment consists of three containers based on hello Docker image for hello Nginx web server.</span></span>

    ```console
    kubectl run mynginx --replicas=3 --image nginx
    ```
2. <span data-ttu-id="e6c8b-127">Ověřte, zda jsou spuštěny hello kontejnery.</span><span class="sxs-lookup"><span data-stu-id="e6c8b-127">Verify that hello containers are running.</span></span> <span data-ttu-id="e6c8b-128">Například, pokud dotaz pro kontejnery hello s `kubectl get pods`, uvidíte výstup podobný toohello následující:</span><span class="sxs-lookup"><span data-stu-id="e6c8b-128">For example, if you query for hello containers with `kubectl get pods`, you see output similar toohello following:</span></span>

    ![Získat nové kontejnery Nginx](./media/container-service-kubernetes-load-balancing/nginx-get-pods.png)

3. <span data-ttu-id="e6c8b-130">tooconfigure hello zatížení vyrovnávání tooaccept externích přenosů toohello nasazení, spusťte `kubectl expose` s `--type=LoadBalancer`.</span><span class="sxs-lookup"><span data-stu-id="e6c8b-130">tooconfigure hello load balancer tooaccept external traffic toohello deployment, run `kubectl expose` with `--type=LoadBalancer`.</span></span> <span data-ttu-id="e6c8b-131">Hello následující příkaz zpřístupní hello Nginx server na portu 80:</span><span class="sxs-lookup"><span data-stu-id="e6c8b-131">hello following command exposes hello Nginx server on port 80:</span></span>

    ```console
    kubectl expose deployments mynginx --port=80 --type=LoadBalancer
    ```

4. <span data-ttu-id="e6c8b-132">Typ `kubectl get svc` toosee hello stavu služby hello v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="e6c8b-132">Type `kubectl get svc` toosee hello state of hello services in hello cluster.</span></span> <span data-ttu-id="e6c8b-133">Při vyrovnávání zatížení hello nakonfiguruje hello pravidlo, hello `EXTERNAL-IP` z hello služby se zobrazí jako `<pending>`.</span><span class="sxs-lookup"><span data-stu-id="e6c8b-133">While hello load balancer configures hello rule, hello `EXTERNAL-IP` of hello service appears as `<pending>`.</span></span> <span data-ttu-id="e6c8b-134">Po několika minutách je nakonfigurované hello externí IP adresu:</span><span class="sxs-lookup"><span data-stu-id="e6c8b-134">After a few minutes, hello external IP address is configured:</span></span> 

    ![Konfigurace pro vyrovnávání zatížení Azure](./media/container-service-kubernetes-load-balancing/nginx-external-ip.png)

5. <span data-ttu-id="e6c8b-136">Ověřte, že máte přístup hello služby hello externí IP adresu.</span><span class="sxs-lookup"><span data-stu-id="e6c8b-136">Verify that you can access hello service at hello external IP address.</span></span> <span data-ttu-id="e6c8b-137">Například otevřete webovou prohlížeče toohello IP adresu vidět.</span><span class="sxs-lookup"><span data-stu-id="e6c8b-137">For example, open a web browser toohello IP address shown.</span></span> <span data-ttu-id="e6c8b-138">Hello prohlížeč zobrazí hello Nginx webový server spuštěný v jednom z kontejnerů hello.</span><span class="sxs-lookup"><span data-stu-id="e6c8b-138">hello browser shows hello Nginx web server running in one of hello containers.</span></span> <span data-ttu-id="e6c8b-139">Nebo spusťte hello `curl` nebo `wget` příkaz.</span><span class="sxs-lookup"><span data-stu-id="e6c8b-139">Or, run hello `curl` or `wget` command.</span></span> <span data-ttu-id="e6c8b-140">Například:</span><span class="sxs-lookup"><span data-stu-id="e6c8b-140">For example:</span></span>

    ```
    curl 13.82.93.130
    ```

    <span data-ttu-id="e6c8b-141">Zobrazený výstup by měl vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="e6c8b-141">You should see output similar to:</span></span>

    ![Nginx přístup pomocí curl](./media/container-service-kubernetes-load-balancing/curl-output.png)

6. <span data-ttu-id="e6c8b-143">toosee hello konfigurace nástroje pro vyrovnávání zatížení Azure hello, přejděte toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="e6c8b-143">toosee hello configuration of hello Azure load balancer, go toohello [Azure portal](https://portal.azure.com).</span></span>

7. <span data-ttu-id="e6c8b-144">Vyhledejte hello skupiny prostředků pro váš cluster container service a zvolte hello Vyrovnávání zatížení pro agenta hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="e6c8b-144">Browse for hello resource group for your container service cluster, and select hello load balancer for hello agent VMs.</span></span> <span data-ttu-id="e6c8b-145">Stejné jako hello kontejneru služby by měl hello její název.</span><span class="sxs-lookup"><span data-stu-id="e6c8b-145">Its name should be hello same as hello container service.</span></span> <span data-ttu-id="e6c8b-146">(Není zvolte hello Vyrovnávání zatížení pro hello řídicí uzly, jeden jejíž název obsahuje hello **hlavní lb**.)</span><span class="sxs-lookup"><span data-stu-id="e6c8b-146">(Don't choose hello load balancer for hello master nodes, hello one whose name includes **master-lb**.)</span></span> 

    ![Nástroje pro vyrovnávání zatížení ve skupině prostředků](./media/container-service-kubernetes-load-balancing/container-resource-group-portal.png)

8. <span data-ttu-id="e6c8b-148">Klikněte na tlačítko Podrobnosti hello toosee hello konfigurace služby Vyrovnávání zatížení, **pravidla Vyrovnávání zatížení** a hello název hello pravidla, která byla nakonfigurována.</span><span class="sxs-lookup"><span data-stu-id="e6c8b-148">toosee hello details of hello load balancer configuration, click **Load balancing rules** and hello name of hello rule that was configured.</span></span>

    ![Pravidla nástroje pro vyrovnávání zatížení](./media/container-service-kubernetes-load-balancing/load-balancing-rules.png) 

### <a name="example-specify-type-loadbalancer-in-hello-service-configuration-file"></a><span data-ttu-id="e6c8b-150">Příklad: Zadejte `type: LoadBalancer` v konfiguračním souboru služby hello</span><span class="sxs-lookup"><span data-stu-id="e6c8b-150">Example: Specify `type: LoadBalancer` in hello service configuration file</span></span>

<span data-ttu-id="e6c8b-151">Pokud nasadíte aplikaci kontejneru Kubernetes z YAML nebo JSON [konfigurační soubor služby](https://kubernetes.io/docs/user-guide/services/operations/#service-configuration-file), zadejte externím vyrovnáváním zatížení přidáním hello následující specifikace service toohello řádku:</span><span class="sxs-lookup"><span data-stu-id="e6c8b-151">If you deploy a Kubernetes container app from a YAML or JSON [service configuration file](https://kubernetes.io/docs/user-guide/services/operations/#service-configuration-file), specify an external load balancer by adding hello following line toohello service specification:</span></span>

```YAML
 "type": "LoadBalancer"
``` 



<span data-ttu-id="e6c8b-152">Hello následující postup použijte hello Kubernetes [návštěv příklad](https://github.com/kubernetes/kubernetes/tree/master/examples/guestbook).</span><span class="sxs-lookup"><span data-stu-id="e6c8b-152">hello following steps use hello Kubernetes [Guestbook example](https://github.com/kubernetes/kubernetes/tree/master/examples/guestbook).</span></span> <span data-ttu-id="e6c8b-153">V tomto příkladu je vícevrstvé webové aplikace založené na imagích Redis a PHP Docker.</span><span class="sxs-lookup"><span data-stu-id="e6c8b-153">This example is a multi-tier web app based on  Redis and PHP Docker images.</span></span> <span data-ttu-id="e6c8b-154">Můžete zadat v konfiguračním souboru služby hello že serveru PHP front-endu hello používá pro vyrovnávání zatížení Azure hello.</span><span class="sxs-lookup"><span data-stu-id="e6c8b-154">You can specify in hello service configuration file that hello frontend PHP server uses hello Azure load balancer.</span></span>

1. <span data-ttu-id="e6c8b-155">Stáhněte si soubor hello `guestbook-all-in-one.yaml` z [Githubu](https://github.com/kubernetes/kubernetes/tree/master/examples/guestbook/all-in-one).</span><span class="sxs-lookup"><span data-stu-id="e6c8b-155">Download hello file `guestbook-all-in-one.yaml` from [GitHub](https://github.com/kubernetes/kubernetes/tree/master/examples/guestbook/all-in-one).</span></span> 
2. <span data-ttu-id="e6c8b-156">Vyhledejte hello `spec` pro hello `frontend` služby.</span><span class="sxs-lookup"><span data-stu-id="e6c8b-156">Browse for hello `spec` for hello `frontend` service.</span></span>
3. <span data-ttu-id="e6c8b-157">Zrušením komentáře u hello řádku `type: LoadBalancer`.</span><span class="sxs-lookup"><span data-stu-id="e6c8b-157">Uncomment hello line `type: LoadBalancer`.</span></span>

    ![Nástroje pro vyrovnávání zatížení v konfiguraci služby](./media/container-service-kubernetes-load-balancing/guestbook-frontend-loadbalance.png)

4. <span data-ttu-id="e6c8b-159">Uložte soubor hello a nasazení aplikace hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e6c8b-159">Save hello file, and deploy hello app by running hello following command:</span></span>

    ```
    kubectl create -f guestbook-all-in-one.yaml
    ```

5. <span data-ttu-id="e6c8b-160">Typ `kubectl get svc` toosee hello stavu služby hello v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="e6c8b-160">Type `kubectl get svc` toosee hello state of hello services in hello cluster.</span></span> <span data-ttu-id="e6c8b-161">Při vyrovnávání zatížení hello nakonfiguruje hello pravidlo, hello `EXTERNAL-IP` z hello `frontend` služby se zobrazí jako `<pending>`.</span><span class="sxs-lookup"><span data-stu-id="e6c8b-161">While hello load balancer configures hello rule, hello `EXTERNAL-IP` of hello `frontend` service appears as `<pending>`.</span></span> <span data-ttu-id="e6c8b-162">Po několika minutách je nakonfigurované hello externí IP adresu:</span><span class="sxs-lookup"><span data-stu-id="e6c8b-162">After a few minutes, hello external IP address is configured:</span></span> 

    ![Konfigurace pro vyrovnávání zatížení Azure](./media/container-service-kubernetes-load-balancing/guestbook-external-ip.png)

6. <span data-ttu-id="e6c8b-164">Ověřte, že máte přístup hello služby hello externí IP adresu.</span><span class="sxs-lookup"><span data-stu-id="e6c8b-164">Verify that you can access hello service at hello external IP address.</span></span> <span data-ttu-id="e6c8b-165">Například můžete otevřít webový prohlížeč toohello externí adresu IP hello služby.</span><span class="sxs-lookup"><span data-stu-id="e6c8b-165">For example, you can open a web browser toohello external IP address of hello service.</span></span>

    ![Externě návštěv přístup](./media/container-service-kubernetes-load-balancing/guestbook-web.png)

    <span data-ttu-id="e6c8b-167">Můžete přidat návštěv položky.</span><span class="sxs-lookup"><span data-stu-id="e6c8b-167">You can add guestbook entries.</span></span>

7. <span data-ttu-id="e6c8b-168">toosee hello konfigurace nástroje pro vyrovnávání zatížení Azure hello, vyhledat hello prostředek pro vyrovnávání zatížení pro cluster hello v hello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="e6c8b-168">toosee hello configuration of hello Azure load balancer, browse for hello load balancer resource for hello cluster in hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="e6c8b-169">Zjistit hello kroků v předchozím příkladu hello.</span><span class="sxs-lookup"><span data-stu-id="e6c8b-169">See hello steps in hello previous example.</span></span>

### <a name="considerations"></a><span data-ttu-id="e6c8b-170">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e6c8b-170">Considerations</span></span>

* <span data-ttu-id="e6c8b-171">Vytvoření pravidla pro vyrovnávání zatížení hello probíhá asynchronně, a informace o vyrovnávání hello zřízený je publikována ve službě hello `status.loadBalancer` pole.</span><span class="sxs-lookup"><span data-stu-id="e6c8b-171">Creation of hello load balancer rule happens asynchronously, and information about hello provisioned balancer is published in hello service’s `status.loadBalancer` field.</span></span>
* <span data-ttu-id="e6c8b-172">Všechny služby se automaticky přiřadí svůj vlastní virtuální IP adresu nástroji pro vyrovnávání zatížení hello.</span><span class="sxs-lookup"><span data-stu-id="e6c8b-172">Every service is automatically assigned its own virtual IP address in hello load balancer.</span></span>
* <span data-ttu-id="e6c8b-173">Pokud chcete nástroj pro vyrovnávání zatížení tooreach hello podle názvu DNS, spolupracovat s vaší domény služby zprostředkovatele toocreate název DNS pro pravidlo hello IP adresu.</span><span class="sxs-lookup"><span data-stu-id="e6c8b-173">If you want tooreach hello load balancer by a DNS name, work with your domain service provider toocreate a DNS name for hello rule's IP address.</span></span>

## <a name="http-or-https-traffic"></a><span data-ttu-id="e6c8b-174">Provoz protokolu HTTP nebo HTTPS</span><span class="sxs-lookup"><span data-stu-id="e6c8b-174">HTTP or HTTPS traffic</span></span>

<span data-ttu-id="e6c8b-175">tooload zůstatek HTTP nebo HTTPS provoz toocontainer webové aplikace a spravovat certifikáty pro (TLS) transport layer security, můžete použít hello Kubernetes [příjem příchozích dat](https://kubernetes.io/docs/user-guide/ingress/) prostředků.</span><span class="sxs-lookup"><span data-stu-id="e6c8b-175">tooload balance HTTP or HTTPS traffic toocontainer web apps and manage certificates for transport layer security (TLS), you can use hello Kubernetes [Ingress](https://kubernetes.io/docs/user-guide/ingress/) resource.</span></span> <span data-ttu-id="e6c8b-176">Příjem příchozích dat je kolekce pravidel, které umožňují příchozí připojení tooreach hello Clusterovou službou.</span><span class="sxs-lookup"><span data-stu-id="e6c8b-176">An Ingress is a collection of rules that allow inbound connections tooreach hello cluster services.</span></span> <span data-ttu-id="e6c8b-177">Prostředku toowork příjem příchozích dat, musí mít hello Kubernetes cluster [příjem příchozích dat řadiče](https://kubernetes.io/docs/user-guide/ingress/#ingress-controllers) systémem.</span><span class="sxs-lookup"><span data-stu-id="e6c8b-177">For an Ingress resource toowork, hello Kubernetes cluster must have an [Ingress controller](https://kubernetes.io/docs/user-guide/ingress/#ingress-controllers) running.</span></span>

<span data-ttu-id="e6c8b-178">Azure Container Service neimplementuje řadič Kubernetes příchozího automaticky.</span><span class="sxs-lookup"><span data-stu-id="e6c8b-178">Azure Container Service does not implement a Kubernetes Ingress controller automatically.</span></span> <span data-ttu-id="e6c8b-179">Několik implementace řadiče jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="e6c8b-179">Several controller implementations are available.</span></span> <span data-ttu-id="e6c8b-180">V současné době hello [Nginx příchozího řadiče](https://github.com/kubernetes/ingress/tree/master/examples/deployment/nginx) se doporučuje tooconfigure příjem příchozích dat pravidel a vyrovnávání zatížení přenosů dat HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="e6c8b-180">Currently, hello [Nginx Ingress controller](https://github.com/kubernetes/ingress/tree/master/examples/deployment/nginx) is recommended tooconfigure Ingress rules and load balance HTTP and HTTPS traffic.</span></span> 

<span data-ttu-id="e6c8b-181">Další informace najdete v tématu hello [dokumentace řadiče příjem příchozích dat Nginx](https://github.com/kubernetes/ingress/tree/master/controllers/nginx/README.md).</span><span class="sxs-lookup"><span data-stu-id="e6c8b-181">For more information, see hello [Nginx Ingress controller documentation](https://github.com/kubernetes/ingress/tree/master/controllers/nginx/README.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e6c8b-182">Při použití hello řadiče Nginx příjem příchozích dat v Azure Container Service, musí vystavit nasazení řadiče hello jako službu s `type: LoadBalancer`.</span><span class="sxs-lookup"><span data-stu-id="e6c8b-182">When using hello Nginx Ingress controller in Azure Container Service, you must expose hello controller deployment as a service with `type: LoadBalancer`.</span></span> <span data-ttu-id="e6c8b-183">Tím se nakonfiguruje hello zatížení Azure vyrovnávání tooroute provoz toohello řadiče.</span><span class="sxs-lookup"><span data-stu-id="e6c8b-183">This configures hello Azure load balancer tooroute traffic toohello controller.</span></span> <span data-ttu-id="e6c8b-184">Další informace najdete v předchozí části hello.</span><span class="sxs-lookup"><span data-stu-id="e6c8b-184">For more information, see hello previous section.</span></span>


## <a name="next-steps"></a><span data-ttu-id="e6c8b-185">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e6c8b-185">Next steps</span></span>

* <span data-ttu-id="e6c8b-186">V tématu hello [dokumentace Kubernetes nástroj pro vyrovnávání zatížení](https://kubernetes.io/docs/user-guide/load-balancer/)</span><span class="sxs-lookup"><span data-stu-id="e6c8b-186">See hello [Kubernetes LoadBalancer documentation](https://kubernetes.io/docs/user-guide/load-balancer/)</span></span>
* <span data-ttu-id="e6c8b-187">Další informace o [řadiče Kubernetes příchozí a příjem příchozích dat](https://kubernetes.io/docs/user-guide/ingress/)</span><span class="sxs-lookup"><span data-stu-id="e6c8b-187">Learn more about [Kubernetes Ingress and Ingress controllers](https://kubernetes.io/docs/user-guide/ingress/)</span></span>
* <span data-ttu-id="e6c8b-188">V tématu [Kubernetes příklady](https://github.com/kubernetes/kubernetes/tree/master/examples)</span><span class="sxs-lookup"><span data-stu-id="e6c8b-188">See [Kubernetes examples](https://github.com/kubernetes/kubernetes/tree/master/examples)</span></span>

