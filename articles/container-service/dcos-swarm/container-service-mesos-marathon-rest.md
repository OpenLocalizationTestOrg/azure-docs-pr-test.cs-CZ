---
title: Spravovat cluster Azure DC/OS s Marathon REST API | Microsoft Docs
description: "Nasazení kontejnerů do clusteru Azure Container Service DC/OS pomocí rozhraní REST API Marathonu."
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, kontejnery, mikroslužby, Mesos, Azure"
ms.assetid: c7175446-4507-4a33-a7a2-63583e5996e3
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/04/2017
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: 65f8e0170fa7b89162e811a1d5dd58775fd20d7b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="dcos-container-management-through-the-marathon-rest-api"></a><span data-ttu-id="419b7-104">Správa kontejnerů DC/OS přes rozhraní REST API Marathonu</span><span class="sxs-lookup"><span data-stu-id="419b7-104">DC/OS container management through the Marathon REST API</span></span>
<span data-ttu-id="419b7-105">DC/OS poskytuje prostředí pro nasazování a škálování clusterových úloh a zároveň poskytuje abstrakci používaného hardwaru.</span><span class="sxs-lookup"><span data-stu-id="419b7-105">DC/OS provides an environment for deploying and scaling clustered workloads, while abstracting the underlying hardware.</span></span> <span data-ttu-id="419b7-106">Nad DC/OS je rozhraní, které spravuje plánování a provádění výpočetních úloh.</span><span class="sxs-lookup"><span data-stu-id="419b7-106">On top of DC/OS, there is a framework that manages scheduling and executing compute workloads.</span></span> <span data-ttu-id="419b7-107">I když jsou k dispozici pro mnoho populárních úloh rozhraní, tento dokument vám pomůže začít vytváření a škálování nasazení kontejnerů pomocí rozhraní REST API Marathonu.</span><span class="sxs-lookup"><span data-stu-id="419b7-107">Although frameworks are available for many popular workloads, this document gets you started creating and scaling container deployments by using the Marathon REST API.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="419b7-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="419b7-108">Prerequisites</span></span>

<span data-ttu-id="419b7-109">Než si projdete tyto příklady, budete potřebovat cluster DC/OS nakonfigurovaný v Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="419b7-109">Before working through these examples, you need a DC/OS cluster that is configured in Azure Container Service.</span></span> <span data-ttu-id="419b7-110">Kromě toho je nutné mít možnost se k tomuto clusteru připojit vzdáleně.</span><span class="sxs-lookup"><span data-stu-id="419b7-110">You also need to have remote connectivity to this cluster.</span></span> <span data-ttu-id="419b7-111">Další informace k těmto záležitostem najdete v těchto článcích:</span><span class="sxs-lookup"><span data-stu-id="419b7-111">For more information on these items, see the following articles:</span></span>

* [<span data-ttu-id="419b7-112">Nasazení clusteru Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="419b7-112">Deploying an Azure Container Service cluster</span></span>](container-service-deployment.md)
* [<span data-ttu-id="419b7-113">Připojení ke clusteru Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="419b7-113">Connecting to an Azure Container Service cluster</span></span>](../container-service-connect.md)

## <a name="access-the-dcos-apis"></a><span data-ttu-id="419b7-114">Přístup k rozhraní API DC/OS</span><span class="sxs-lookup"><span data-stu-id="419b7-114">Access the DC/OS APIs</span></span>
<span data-ttu-id="419b7-115">Až se připojíte ke clusteru Azure Container Service, budete mít na DC/OS a související rozhraní REST API přístup přes adresu http://localhost:local-port.</span><span class="sxs-lookup"><span data-stu-id="419b7-115">After you are connected to the Azure Container Service cluster, you can access the DC/OS and related REST APIs through http://localhost:local-port.</span></span> <span data-ttu-id="419b7-116">Příklady v tomto dokumentu předpokládají, že máte k dispozici tunel na portu 80.</span><span class="sxs-lookup"><span data-stu-id="419b7-116">The examples in this document assume that you are tunneling on port 80.</span></span> <span data-ttu-id="419b7-117">Například Marathon koncové body k dispozici na adrese identifikátory URI počínaje `http://localhost/marathon/v2/`.</span><span class="sxs-lookup"><span data-stu-id="419b7-117">For example, the Marathon endpoints can be reached at URIs beginning with `http://localhost/marathon/v2/`.</span></span> 

<span data-ttu-id="419b7-118">Další informace o různých rozhraních API najdete v dokumentaci Mesosphere pro rozhraní [Marathon API](https://mesosphere.github.io/marathon/docs/rest-api.html) a [Chronos API](https://mesos.github.io/chronos/docs/api.html) a v dokumentaci Apache pro rozhraní [Mesos Scheduler API](http://mesos.apache.org/documentation/latest/scheduler-http-api/).</span><span class="sxs-lookup"><span data-stu-id="419b7-118">For more information on the various APIs, see the Mesosphere documentation for the [Marathon API](https://mesosphere.github.io/marathon/docs/rest-api.html) and the [Chronos API](https://mesos.github.io/chronos/docs/api.html), and the Apache documentation for the [Mesos Scheduler API](http://mesos.apache.org/documentation/latest/scheduler-http-api/).</span></span>

## <a name="gather-information-from-dcos-and-marathon"></a><span data-ttu-id="419b7-119">Získání informací z DC/OS a Marathonu</span><span class="sxs-lookup"><span data-stu-id="419b7-119">Gather information from DC/OS and Marathon</span></span>
<span data-ttu-id="419b7-120">Než do clusteru DC/OS nasadíte kontejnery, zjistěte si určité informace o clusteru DC/OS, například názvy a stav agentů DC/OS.</span><span class="sxs-lookup"><span data-stu-id="419b7-120">Before you deploy containers to the DC/OS cluster, gather some information about the DC/OS cluster, such as the names and status of the DC/OS agents.</span></span> <span data-ttu-id="419b7-121">To provedete tak, že zašlete dotaz na koncový bod `master/slaves` rozhraní REST API DC/OS.</span><span class="sxs-lookup"><span data-stu-id="419b7-121">To do so, query the `master/slaves` endpoint of the DC/OS REST API.</span></span> <span data-ttu-id="419b7-122">Pokud všechno proběhne správně, dotaz vrátí seznam agentů DC/OS a u každého z nich několik vlastností.</span><span class="sxs-lookup"><span data-stu-id="419b7-122">If everything goes well, the query returns a list of DC/OS agents and several properties for each.</span></span>

```bash
curl http://localhost/mesos/master/slaves
```

<span data-ttu-id="419b7-123">Nyní pomocí koncového bodu Marathon `/apps` zkontrolujte aktuální nasazení aplikací v clusteru DC/OS.</span><span class="sxs-lookup"><span data-stu-id="419b7-123">Now, use the Marathon `/apps` endpoint to check for current application deployments to the DC/OS cluster.</span></span> <span data-ttu-id="419b7-124">Pokud je to nový cluster, uvidíte prázdné pole aplikací.</span><span class="sxs-lookup"><span data-stu-id="419b7-124">If this is a new cluster, you see an empty array for apps.</span></span>

```bash
curl localhost/marathon/v2/apps

{"apps":[]}
```

## <a name="deploy-a-docker-formatted-container"></a><span data-ttu-id="419b7-125">Nasazení kontejneru formátovaného Dockerem</span><span class="sxs-lookup"><span data-stu-id="419b7-125">Deploy a Docker-formatted container</span></span>
<span data-ttu-id="419b7-126">Kontejnery formátované Dockerem prostřednictvím rozhraní REST API Marathonu nasadíte pomocí souboru JSON, který popisuje zamýšlené nasazení.</span><span class="sxs-lookup"><span data-stu-id="419b7-126">You deploy Docker-formatted containers through the Marathon REST API by using a JSON file that describes the intended deployment.</span></span> <span data-ttu-id="419b7-127">Následující ukázka nasadí kontejner Nginx do privátní agenta v clusteru.</span><span class="sxs-lookup"><span data-stu-id="419b7-127">The following sample deploys an Nginx container to a private agent in the cluster.</span></span> 

```json
{
  "id": "nginx",
  "cpus": 0.1,
  "mem": 32.0,
  "instances": 1,
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        { "containerPort": 80, "servicePort": 9000, "protocol": "tcp" }
      ]
    }
  }
}
```

<span data-ttu-id="419b7-128">Abyste mohli nasadit kontejner formátovaný, ukládat do přístupného umístění souboru JSON.</span><span class="sxs-lookup"><span data-stu-id="419b7-128">To deploy a Docker-formatted container, store the JSON file in an accessible location.</span></span> <span data-ttu-id="419b7-129">Pak následujícím příkazem nasaďte kontejner.</span><span class="sxs-lookup"><span data-stu-id="419b7-129">Next, to deploy the container, run the following command.</span></span> <span data-ttu-id="419b7-130">Zadejte název souboru JSON (`marathon.json` v tomto příkladu).</span><span class="sxs-lookup"><span data-stu-id="419b7-130">Specify the name of the JSON file (`marathon.json` in this example).</span></span>

```bash
curl -X POST http://localhost/marathon/v2/apps -d @marathon.json -H "Content-type: application/json"
```

<span data-ttu-id="419b7-131">Výstup je podobný tomuto:</span><span class="sxs-lookup"><span data-stu-id="419b7-131">The output is similar to the following:</span></span>

```json
{"version":"2015-11-20T18:59:00.494Z","deploymentId":"b12f8a73-f56a-4eb1-9375-4ac026d6cdec"}
```

<span data-ttu-id="419b7-132">Nyní když Marathonu odešlete dotaz na aplikace, zobrazí se tato nová aplikace ve výstupu.</span><span class="sxs-lookup"><span data-stu-id="419b7-132">Now, if you query Marathon for applications, this new application appears in the output.</span></span>

```bash
curl localhost/marathon/v2/apps
```

## <a name="reach-the-container"></a><span data-ttu-id="419b7-133">Dosažení kontejneru</span><span class="sxs-lookup"><span data-stu-id="419b7-133">Reach the container</span></span>

<span data-ttu-id="419b7-134">Můžete ověřit, že Nginx běží v kontejneru na jednom ze soukromých agentů v clusteru.</span><span class="sxs-lookup"><span data-stu-id="419b7-134">You can verify that the Nginx is running in a container on one of the private agents in the cluster.</span></span> <span data-ttu-id="419b7-135">Chcete-li najít hostitele a portu, na kterém je spuštěný kontejneru, dotaz Marathon pro spuštěné úlohy:</span><span class="sxs-lookup"><span data-stu-id="419b7-135">To find the host and port where the container is running, query Marathon for the running tasks:</span></span> 

```bash
curl localhost/marathon/v2/tasks
```

<span data-ttu-id="419b7-136">Vrátí hodnotu `host` ve výstupu (podobně jako IP adresu `10.32.0.x`) a hodnota `ports`.</span><span class="sxs-lookup"><span data-stu-id="419b7-136">Find the value of `host` in the output (an IP address similar to `10.32.0.x`), and the value of `ports`.</span></span>


<span data-ttu-id="419b7-137">Nyní terminálu připojení SSH (ne tunelového propojení) proveďte správu plně kvalifikovaný název domény clusteru.</span><span class="sxs-lookup"><span data-stu-id="419b7-137">Now make an SSH terminal connection (not a tunneled connection) to the management FQDN of the cluster.</span></span> <span data-ttu-id="419b7-138">Po připojení, zkontrolujte následující požadavek, nahraďte správné hodnoty z `host` a `ports`:</span><span class="sxs-lookup"><span data-stu-id="419b7-138">Once connected, make the following request, substituting the correct values of `host` and `ports`:</span></span>

```bash
curl http://host:ports
```

<span data-ttu-id="419b7-139">Výstup serveru Nginx je podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="419b7-139">The Nginx server output is similar to the following:</span></span>

![Nginx z kontejneru](./media/container-service-mesos-marathon-rest/nginx.png)




## <a name="scale-your-containers"></a><span data-ttu-id="419b7-141">Škálování kontejnerů</span><span class="sxs-lookup"><span data-stu-id="419b7-141">Scale your containers</span></span>
<span data-ttu-id="419b7-142">Můžete použít rozhraní API Marathonu horizontální navýšení kapacity nebo škálování v nasazení aplikace.</span><span class="sxs-lookup"><span data-stu-id="419b7-142">You can use the Marathon API to scale out or scale in application deployments.</span></span> <span data-ttu-id="419b7-143">V předchozím příkladu jste nasadili jednu instanci aplikace.</span><span class="sxs-lookup"><span data-stu-id="419b7-143">In the previous example, you deployed one instance of an application.</span></span> <span data-ttu-id="419b7-144">Nyní škálování aplikace navyšme na tři instance.</span><span class="sxs-lookup"><span data-stu-id="419b7-144">Let's scale this out to three instances of an application.</span></span> <span data-ttu-id="419b7-145">To provedete tak, že pomocí následujícího textu JSON vytvoříte soubor JSON a uložíte ho na dostupném místě.</span><span class="sxs-lookup"><span data-stu-id="419b7-145">To do so, create a JSON file by using the following JSON text, and store it in an accessible location.</span></span>

```json
{ "instances": 3 }
```

<span data-ttu-id="419b7-146">Z tunelového propojení spusťte následující příkaz pro horizontální škálování aplikace.</span><span class="sxs-lookup"><span data-stu-id="419b7-146">From your tunneled connection, run the following command to scale out the application.</span></span>

> [!NOTE]
> <span data-ttu-id="419b7-147">Identifikátor URI je http://localhost/marathon/v2/apps/ a pak ID aplikace, která se bude škálovat.</span><span class="sxs-lookup"><span data-stu-id="419b7-147">The URI is http://localhost/marathon/v2/apps/ followed by the ID of the application to scale.</span></span> <span data-ttu-id="419b7-148">Pokud používáte ukázku Nginx, která je zde k dispozici, identifikátor URI by byl http://localhost/marathon/v2/apps/nginx.</span><span class="sxs-lookup"><span data-stu-id="419b7-148">If you are using the Nginx sample that is provided here, the URI would be http://localhost/marathon/v2/apps/nginx.</span></span>
> 
> 

```bash
curl http://localhost/marathon/v2/apps/nginx -H "Content-type: application/json" -X PUT -d @scale.json
```

<span data-ttu-id="419b7-149">Nakonec pošlete na koncový bod Marathon dotaz na aplikace.</span><span class="sxs-lookup"><span data-stu-id="419b7-149">Finally, query the Marathon endpoint for applications.</span></span> <span data-ttu-id="419b7-150">Vidíte, že tam jsou nyní tři kontejnery Nginx.</span><span class="sxs-lookup"><span data-stu-id="419b7-150">You see that there are now three Nginx containers.</span></span>

```bash
curl localhost/marathon/v2/apps
```

## <a name="equivalent-powershell-commands"></a><span data-ttu-id="419b7-151">Ekvivalentní příkazy PowerShellu</span><span class="sxs-lookup"><span data-stu-id="419b7-151">Equivalent PowerShell commands</span></span>
<span data-ttu-id="419b7-152">V systému Windows můžete tyto stejné akce provést pomocí příkazů PowerShellu.</span><span class="sxs-lookup"><span data-stu-id="419b7-152">You can perform these same actions by using PowerShell commands on a Windows system.</span></span>

<span data-ttu-id="419b7-153">Získat informace o clusteru DC/OS, jako jsou názvy agentů a stavu agentů, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="419b7-153">To gather information about the DC/OS cluster, such as agent names and agent status, run the following command:</span></span>

```powershell
Invoke-WebRequest -Uri http://localhost/mesos/master/slaves
```

<span data-ttu-id="419b7-154">Kontejnery formátované Dockerem nasadíte přes Marathon pomocí souboru JSON, který popisuje zamýšlené nasazení.</span><span class="sxs-lookup"><span data-stu-id="419b7-154">You deploy Docker-formatted containers through Marathon by using a JSON file that describes the intended deployment.</span></span> <span data-ttu-id="419b7-155">Následující ukázka nasadí kontejner Nginx a sváže port 80 agenta DC/OS s portem 80 kontejneru.</span><span class="sxs-lookup"><span data-stu-id="419b7-155">The following sample deploys the Nginx container, binding port 80 of the DC/OS agent to port 80 of the container.</span></span>

```json
{
  "id": "nginx",
  "cpus": 0.1,
  "mem": 32.0,
  "instances": 1,
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        { "containerPort": 80, "servicePort": 9000, "protocol": "tcp" }
      ]
    }
  }
}
```

<span data-ttu-id="419b7-156">Abyste mohli nasadit kontejner formátovaný, ukládat do přístupného umístění souboru JSON.</span><span class="sxs-lookup"><span data-stu-id="419b7-156">To deploy a Docker-formatted container, store the JSON file in an accessible location.</span></span> <span data-ttu-id="419b7-157">Pak následujícím příkazem nasaďte kontejner.</span><span class="sxs-lookup"><span data-stu-id="419b7-157">Next, to deploy the container, run the following command.</span></span> <span data-ttu-id="419b7-158">Zadejte cestu k souboru JSON (`marathon.json` v tomto příkladu).</span><span class="sxs-lookup"><span data-stu-id="419b7-158">Specify the path to the JSON file (`marathon.json` in this example).</span></span>

```powershell
Invoke-WebRequest -Method Post -Uri http://localhost/marathon/v2/apps -ContentType application/json -InFile 'c:\marathon.json'
```

<span data-ttu-id="419b7-159">Rozhraní Marathon API je možné použít i k nasazením aplikací se škálováním pro horizontální navýšení nebo snížení kapacity.</span><span class="sxs-lookup"><span data-stu-id="419b7-159">You can also use the Marathon API to scale out or scale in application deployments.</span></span> <span data-ttu-id="419b7-160">V předchozím příkladu jste nasadili jednu instanci aplikace.</span><span class="sxs-lookup"><span data-stu-id="419b7-160">In the previous example, you deployed one instance of an application.</span></span> <span data-ttu-id="419b7-161">Nyní škálování aplikace navyšme na tři instance.</span><span class="sxs-lookup"><span data-stu-id="419b7-161">Let's scale this out to three instances of an application.</span></span> <span data-ttu-id="419b7-162">To provedete tak, že pomocí následujícího textu JSON vytvoříte soubor JSON a uložíte ho na dostupném místě.</span><span class="sxs-lookup"><span data-stu-id="419b7-162">To do so, create a JSON file by using the following JSON text, and store it in an accessible location.</span></span>

```json
{ "instances": 3 }
```

<span data-ttu-id="419b7-163">Spusťte následující příkaz pro horizontální škálování aplikace:</span><span class="sxs-lookup"><span data-stu-id="419b7-163">Run the following command to scale out the application:</span></span>

> [!NOTE]
> <span data-ttu-id="419b7-164">Identifikátor URI je http://localhost/marathon/v2/apps/ a pak ID aplikace, která se bude škálovat.</span><span class="sxs-lookup"><span data-stu-id="419b7-164">The URI is http://localhost/marathon/v2/apps/ followed by the ID of the application to scale.</span></span> <span data-ttu-id="419b7-165">Pokud používáte ukázku Nginx, která je zde k dispozici, identifikátor URI by byl http://localhost/marathon/v2/apps/nginx.</span><span class="sxs-lookup"><span data-stu-id="419b7-165">If you are using the Nginx sample provided here, the URI would be http://localhost/marathon/v2/apps/nginx.</span></span>
> 
> 

```powershell
Invoke-WebRequest -Method Put -Uri http://localhost/marathon/v2/apps/nginx -ContentType application/json -InFile 'c:\scale.json'
```

## <a name="next-steps"></a><span data-ttu-id="419b7-166">Další kroky</span><span class="sxs-lookup"><span data-stu-id="419b7-166">Next steps</span></span>
* [<span data-ttu-id="419b7-167">Další informace o koncových bodech Mesos HTTP</span><span class="sxs-lookup"><span data-stu-id="419b7-167">Read more about the Mesos HTTP endpoints</span></span>](http://mesos.apache.org/documentation/latest/endpoints/)
* [<span data-ttu-id="419b7-168">Další informace o rozhraní REST API Marathonu</span><span class="sxs-lookup"><span data-stu-id="419b7-168">Read more about the Marathon REST API</span></span>](https://mesosphere.github.io/marathon/docs/rest-api.html)

