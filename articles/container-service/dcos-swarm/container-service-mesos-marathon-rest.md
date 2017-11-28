---
title: aaaManage Azure DC/OS cluster s Marathon REST API | Microsoft Docs
description: "Nasazení clusteru Azure Container Service DC/OS tooan kontejnery pomocí rozhraní REST API Marathonu hello."
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
ms.openlocfilehash: d926b9b90f5d4eda85a015d9ea0d96fea2c4b566
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="dcos-container-management-through-hello-marathon-rest-api"></a><span data-ttu-id="62bac-104">Správa kontejnerů DC/OS prostřednictvím hello rozhraní REST API Marathonu</span><span class="sxs-lookup"><span data-stu-id="62bac-104">DC/OS container management through hello Marathon REST API</span></span>
<span data-ttu-id="62bac-105">DC/OS poskytuje prostředí pro nasazování a škálování clusterových úloh a zároveň poskytuje abstrakci používaného hardwaru hello.</span><span class="sxs-lookup"><span data-stu-id="62bac-105">DC/OS provides an environment for deploying and scaling clustered workloads, while abstracting hello underlying hardware.</span></span> <span data-ttu-id="62bac-106">Nad DC/OS je rozhraní, které spravuje plánování a provádění výpočetních úloh.</span><span class="sxs-lookup"><span data-stu-id="62bac-106">On top of DC/OS, there is a framework that manages scheduling and executing compute workloads.</span></span> <span data-ttu-id="62bac-107">I když jsou k dispozici pro mnoho populárních úloh rozhraní, tento dokument vám pomůže začít vytváření a škálování nasazení kontejnerů pomocí hello Marathon REST API.</span><span class="sxs-lookup"><span data-stu-id="62bac-107">Although frameworks are available for many popular workloads, this document gets you started creating and scaling container deployments by using hello Marathon REST API.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="62bac-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="62bac-108">Prerequisites</span></span>

<span data-ttu-id="62bac-109">Než si projdete tyto příklady, budete potřebovat cluster DC/OS nakonfigurovaný v Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="62bac-109">Before working through these examples, you need a DC/OS cluster that is configured in Azure Container Service.</span></span> <span data-ttu-id="62bac-110">Budete také potřebovat clusteru toothis toohave vzdáleného připojení.</span><span class="sxs-lookup"><span data-stu-id="62bac-110">You also need toohave remote connectivity toothis cluster.</span></span> <span data-ttu-id="62bac-111">Další informace o těchto položek najdete v tématu hello následující články:</span><span class="sxs-lookup"><span data-stu-id="62bac-111">For more information on these items, see hello following articles:</span></span>

* [<span data-ttu-id="62bac-112">Nasazení clusteru Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="62bac-112">Deploying an Azure Container Service cluster</span></span>](container-service-deployment.md)
* [<span data-ttu-id="62bac-113">Připojení clusteru Azure Container Service tooan</span><span class="sxs-lookup"><span data-stu-id="62bac-113">Connecting tooan Azure Container Service cluster</span></span>](../container-service-connect.md)

## <a name="access-hello-dcos-apis"></a><span data-ttu-id="62bac-114">Přístup k rozhraní API DC/OS hello</span><span class="sxs-lookup"><span data-stu-id="62bac-114">Access hello DC/OS APIs</span></span>
<span data-ttu-id="62bac-115">Jakmile se připojené toohello clusteru Azure Container Service můžete přistupovat hello DC/OS a související rozhraní REST API přes adresu http://localhost: Local-port.</span><span class="sxs-lookup"><span data-stu-id="62bac-115">After you are connected toohello Azure Container Service cluster, you can access hello DC/OS and related REST APIs through http://localhost:local-port.</span></span> <span data-ttu-id="62bac-116">Hello příklady v tomto dokumentu předpokládají, že máte k dispozici tunel na portu 80.</span><span class="sxs-lookup"><span data-stu-id="62bac-116">hello examples in this document assume that you are tunneling on port 80.</span></span> <span data-ttu-id="62bac-117">Například hello Marathon koncové body k dispozici na adrese identifikátory URI počínaje `http://localhost/marathon/v2/`.</span><span class="sxs-lookup"><span data-stu-id="62bac-117">For example, hello Marathon endpoints can be reached at URIs beginning with `http://localhost/marathon/v2/`.</span></span> 

<span data-ttu-id="62bac-118">Další informace o hello různých rozhraních API, najdete v dokumentaci Mesosphere hello hello [Marathon API](https://mesosphere.github.io/marathon/docs/rest-api.html) a [Chronos API](https://mesos.github.io/chronos/docs/api.html)a v dokumentaci Apache pro hello [Mesos Scheduler API ](http://mesos.apache.org/documentation/latest/scheduler-http-api/).</span><span class="sxs-lookup"><span data-stu-id="62bac-118">For more information on hello various APIs, see hello Mesosphere documentation for hello [Marathon API](https://mesosphere.github.io/marathon/docs/rest-api.html) and the [Chronos API](https://mesos.github.io/chronos/docs/api.html), and the Apache documentation for hello [Mesos Scheduler API](http://mesos.apache.org/documentation/latest/scheduler-http-api/).</span></span>

## <a name="gather-information-from-dcos-and-marathon"></a><span data-ttu-id="62bac-119">Získání informací z DC/OS a Marathonu</span><span class="sxs-lookup"><span data-stu-id="62bac-119">Gather information from DC/OS and Marathon</span></span>
<span data-ttu-id="62bac-120">Před nasazením clusteru DC/OS toohello kontejnery, zjistěte si určité informace o clusteru DC/OS hello, například názvy hello a stav agentů DC/OS hello.</span><span class="sxs-lookup"><span data-stu-id="62bac-120">Before you deploy containers toohello DC/OS cluster, gather some information about hello DC/OS cluster, such as hello names and status of hello DC/OS agents.</span></span> <span data-ttu-id="62bac-121">toodo tedy dotaz hello `master/slaves` koncový bod hello REST API DC/OS.</span><span class="sxs-lookup"><span data-stu-id="62bac-121">toodo so, query hello `master/slaves` endpoint of hello DC/OS REST API.</span></span> <span data-ttu-id="62bac-122">Pokud všechno půjde dobře, hello dotaz vrátí seznam agentů DC/OS a několik vlastností, které pro každý.</span><span class="sxs-lookup"><span data-stu-id="62bac-122">If everything goes well, hello query returns a list of DC/OS agents and several properties for each.</span></span>

```bash
curl http://localhost/mesos/master/slaves
```

<span data-ttu-id="62bac-123">Nyní pomocí Marathonu hello `/apps` toocheck koncový bod pro aktuální clusteru DC/OS toohello nasazení aplikace.</span><span class="sxs-lookup"><span data-stu-id="62bac-123">Now, use hello Marathon `/apps` endpoint toocheck for current application deployments toohello DC/OS cluster.</span></span> <span data-ttu-id="62bac-124">Pokud je to nový cluster, uvidíte prázdné pole aplikací.</span><span class="sxs-lookup"><span data-stu-id="62bac-124">If this is a new cluster, you see an empty array for apps.</span></span>

```bash
curl localhost/marathon/v2/apps

{"apps":[]}
```

## <a name="deploy-a-docker-formatted-container"></a><span data-ttu-id="62bac-125">Nasazení kontejneru formátovaného Dockerem</span><span class="sxs-lookup"><span data-stu-id="62bac-125">Deploy a Docker-formatted container</span></span>
<span data-ttu-id="62bac-126">Kontejnery formátované Dockerem prostřednictvím hello rozhraní REST API Marathonu nasadíte pomocí souboru JSON, který popisuje hello určený nasazení.</span><span class="sxs-lookup"><span data-stu-id="62bac-126">You deploy Docker-formatted containers through hello Marathon REST API by using a JSON file that describes hello intended deployment.</span></span> <span data-ttu-id="62bac-127">Hello následující ukázka nasadí Nginx kontejneru tooa privátní agenta v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="62bac-127">hello following sample deploys an Nginx container tooa private agent in hello cluster.</span></span> 

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

<span data-ttu-id="62bac-128">toodeploy kontejner formátovaný hello JSON soubor uložte do přístupného umístění.</span><span class="sxs-lookup"><span data-stu-id="62bac-128">toodeploy a Docker-formatted container, store hello JSON file in an accessible location.</span></span> <span data-ttu-id="62bac-129">Dále toodeploy hello kontejner, spusťte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="62bac-129">Next, toodeploy hello container, run hello following command.</span></span> <span data-ttu-id="62bac-130">Zadejte název souboru JSON hello hello (`marathon.json` v tomto příkladu).</span><span class="sxs-lookup"><span data-stu-id="62bac-130">Specify hello name of hello JSON file (`marathon.json` in this example).</span></span>

```bash
curl -X POST http://localhost/marathon/v2/apps -d @marathon.json -H "Content-type: application/json"
```

<span data-ttu-id="62bac-131">výstup Hello je podobné toohello následující:</span><span class="sxs-lookup"><span data-stu-id="62bac-131">hello output is similar toohello following:</span></span>

```json
{"version":"2015-11-20T18:59:00.494Z","deploymentId":"b12f8a73-f56a-4eb1-9375-4ac026d6cdec"}
```

<span data-ttu-id="62bac-132">Teď když Marathonu odešlete dotaz pro aplikace, tato nová aplikace se zobrazí ve výstupu hello.</span><span class="sxs-lookup"><span data-stu-id="62bac-132">Now, if you query Marathon for applications, this new application appears in hello output.</span></span>

```bash
curl localhost/marathon/v2/apps
```

## <a name="reach-hello-container"></a><span data-ttu-id="62bac-133">Dosažení hello kontejneru</span><span class="sxs-lookup"><span data-stu-id="62bac-133">Reach hello container</span></span>

<span data-ttu-id="62bac-134">Můžete ověřit, že hello Nginx je spuštěn v kontejneru v jednom z hello privátní agenti v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="62bac-134">You can verify that hello Nginx is running in a container on one of hello private agents in hello cluster.</span></span> <span data-ttu-id="62bac-135">toofind hello hostitele a portu, kde hello kontejneru běží, dotaz Marathon pro hello spuštěných úloh:</span><span class="sxs-lookup"><span data-stu-id="62bac-135">toofind hello host and port where hello container is running, query Marathon for hello running tasks:</span></span> 

```bash
curl localhost/marathon/v2/tasks
```

<span data-ttu-id="62bac-136">Najít hodnotu hello `host` ve výstupu hello (IP adres podobné příliš`10.32.0.x`) a hodnota hello `ports`.</span><span class="sxs-lookup"><span data-stu-id="62bac-136">Find hello value of `host` in hello output (an IP address similar too`10.32.0.x`), and hello value of `ports`.</span></span>


<span data-ttu-id="62bac-137">Nyní proveďte SSH terminálu připojení (ne tunelového propojení) toohello správu plně kvalifikovaný název domény clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="62bac-137">Now make an SSH terminal connection (not a tunneled connection) toohello management FQDN of hello cluster.</span></span> <span data-ttu-id="62bac-138">Po připojení, zkontrolujte hello následující požadavek, nahraďte hello správné hodnoty z `host` a `ports`:</span><span class="sxs-lookup"><span data-stu-id="62bac-138">Once connected, make hello following request, substituting hello correct values of `host` and `ports`:</span></span>

```bash
curl http://host:ports
```

<span data-ttu-id="62bac-139">Hello výstup Nginx serveru je podobné toohello následující:</span><span class="sxs-lookup"><span data-stu-id="62bac-139">hello Nginx server output is similar toohello following:</span></span>

![Nginx z kontejneru](./media/container-service-mesos-marathon-rest/nginx.png)




## <a name="scale-your-containers"></a><span data-ttu-id="62bac-141">Škálování kontejnerů</span><span class="sxs-lookup"><span data-stu-id="62bac-141">Scale your containers</span></span>
<span data-ttu-id="62bac-142">V nasazení aplikace můžete použít hello Marathon API tooscale out nebo určený počet číslic.</span><span class="sxs-lookup"><span data-stu-id="62bac-142">You can use hello Marathon API tooscale out or scale in application deployments.</span></span> <span data-ttu-id="62bac-143">V předchozím příkladu hello jste nasadili jednu instanci aplikace.</span><span class="sxs-lookup"><span data-stu-id="62bac-143">In hello previous example, you deployed one instance of an application.</span></span> <span data-ttu-id="62bac-144">Nyní škálování to zjistit toothree instancí aplikace.</span><span class="sxs-lookup"><span data-stu-id="62bac-144">Let's scale this out toothree instances of an application.</span></span> <span data-ttu-id="62bac-145">toodo tak, vytvořte soubor JSON s použitím hello následující JSON text a uložte ho do přístupného umístění.</span><span class="sxs-lookup"><span data-stu-id="62bac-145">toodo so, create a JSON file by using hello following JSON text, and store it in an accessible location.</span></span>

```json
{ "instances": 3 }
```

<span data-ttu-id="62bac-146">Z tunelového propojení spusťte následující příkaz tooscale se aplikace hello hello.</span><span class="sxs-lookup"><span data-stu-id="62bac-146">From your tunneled connection, run hello following command tooscale out hello application.</span></span>

> [!NOTE]
> <span data-ttu-id="62bac-147">Hello identifikátor URI je http://localhost/marathon/v2/apps/ následuje hello ID tooscale aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="62bac-147">hello URI is http://localhost/marathon/v2/apps/ followed by hello ID of hello application tooscale.</span></span> <span data-ttu-id="62bac-148">Pokud používáte ukázku Nginx hello, která je k dispozici zde, hello URI by byl http://localhost/marathon/v2/apps/nginx.</span><span class="sxs-lookup"><span data-stu-id="62bac-148">If you are using hello Nginx sample that is provided here, hello URI would be http://localhost/marathon/v2/apps/nginx.</span></span>
> 
> 

```bash
curl http://localhost/marathon/v2/apps/nginx -H "Content-type: application/json" -X PUT -d @scale.json
```

<span data-ttu-id="62bac-149">Nakonec dotaz na koncový bod Marathonu hello pro aplikace.</span><span class="sxs-lookup"><span data-stu-id="62bac-149">Finally, query hello Marathon endpoint for applications.</span></span> <span data-ttu-id="62bac-150">Vidíte, že tam jsou nyní tři kontejnery Nginx.</span><span class="sxs-lookup"><span data-stu-id="62bac-150">You see that there are now three Nginx containers.</span></span>

```bash
curl localhost/marathon/v2/apps
```

## <a name="equivalent-powershell-commands"></a><span data-ttu-id="62bac-151">Ekvivalentní příkazy PowerShellu</span><span class="sxs-lookup"><span data-stu-id="62bac-151">Equivalent PowerShell commands</span></span>
<span data-ttu-id="62bac-152">V systému Windows můžete tyto stejné akce provést pomocí příkazů PowerShellu.</span><span class="sxs-lookup"><span data-stu-id="62bac-152">You can perform these same actions by using PowerShell commands on a Windows system.</span></span>

<span data-ttu-id="62bac-153">toogather informace o clusteru DC/OS hello, jako jsou názvy agentů a stavu agentů, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="62bac-153">toogather information about hello DC/OS cluster, such as agent names and agent status, run hello following command:</span></span>

```powershell
Invoke-WebRequest -Uri http://localhost/mesos/master/slaves
```

<span data-ttu-id="62bac-154">Nasadíte kontejnery formátované Dockerem přes Marathon pomocí souboru JSON, který popisuje hello určený nasazení.</span><span class="sxs-lookup"><span data-stu-id="62bac-154">You deploy Docker-formatted containers through Marathon by using a JSON file that describes hello intended deployment.</span></span> <span data-ttu-id="62bac-155">Hello následující ukázka nasadí kontejner nginx a sváže hello vazby port 80 hello DC/OS agenta tooport 80 kontejneru hello.</span><span class="sxs-lookup"><span data-stu-id="62bac-155">hello following sample deploys hello Nginx container, binding port 80 of hello DC/OS agent tooport 80 of hello container.</span></span>

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

<span data-ttu-id="62bac-156">toodeploy kontejner formátovaný hello JSON soubor uložte do přístupného umístění.</span><span class="sxs-lookup"><span data-stu-id="62bac-156">toodeploy a Docker-formatted container, store hello JSON file in an accessible location.</span></span> <span data-ttu-id="62bac-157">Dále toodeploy hello kontejner, spusťte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="62bac-157">Next, toodeploy hello container, run hello following command.</span></span> <span data-ttu-id="62bac-158">Zadejte soubor JSON toohello cesta hello (`marathon.json` v tomto příkladu).</span><span class="sxs-lookup"><span data-stu-id="62bac-158">Specify hello path toohello JSON file (`marathon.json` in this example).</span></span>

```powershell
Invoke-WebRequest -Method Post -Uri http://localhost/marathon/v2/apps -ContentType application/json -InFile 'c:\marathon.json'
```

<span data-ttu-id="62bac-159">V nasazení aplikace můžete použít také hello Marathon API tooscale out nebo škálování.</span><span class="sxs-lookup"><span data-stu-id="62bac-159">You can also use hello Marathon API tooscale out or scale in application deployments.</span></span> <span data-ttu-id="62bac-160">V předchozím příkladu hello jste nasadili jednu instanci aplikace.</span><span class="sxs-lookup"><span data-stu-id="62bac-160">In hello previous example, you deployed one instance of an application.</span></span> <span data-ttu-id="62bac-161">Nyní škálování to zjistit toothree instancí aplikace.</span><span class="sxs-lookup"><span data-stu-id="62bac-161">Let's scale this out toothree instances of an application.</span></span> <span data-ttu-id="62bac-162">toodo tak, vytvořte soubor JSON s použitím hello následující JSON text a uložte ho do přístupného umístění.</span><span class="sxs-lookup"><span data-stu-id="62bac-162">toodo so, create a JSON file by using hello following JSON text, and store it in an accessible location.</span></span>

```json
{ "instances": 3 }
```

<span data-ttu-id="62bac-163">Spusťte následující příkaz tooscale se aplikace hello hello:</span><span class="sxs-lookup"><span data-stu-id="62bac-163">Run hello following command tooscale out hello application:</span></span>

> [!NOTE]
> <span data-ttu-id="62bac-164">Hello identifikátor URI je http://localhost/marathon/v2/apps/ následuje hello ID tooscale aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="62bac-164">hello URI is http://localhost/marathon/v2/apps/ followed by hello ID of hello application tooscale.</span></span> <span data-ttu-id="62bac-165">Pokud používáte ukázku Nginx hello poskytuje zde, hello identifikátoru URI by http://localhost/marathon/v2/apps/nginx.</span><span class="sxs-lookup"><span data-stu-id="62bac-165">If you are using hello Nginx sample provided here, hello URI would be http://localhost/marathon/v2/apps/nginx.</span></span>
> 
> 

```powershell
Invoke-WebRequest -Method Put -Uri http://localhost/marathon/v2/apps/nginx -ContentType application/json -InFile 'c:\scale.json'
```

## <a name="next-steps"></a><span data-ttu-id="62bac-166">Další kroky</span><span class="sxs-lookup"><span data-stu-id="62bac-166">Next steps</span></span>
* [<span data-ttu-id="62bac-167">Další informace o koncových bodech Mesos HTTP hello</span><span class="sxs-lookup"><span data-stu-id="62bac-167">Read more about hello Mesos HTTP endpoints</span></span>](http://mesos.apache.org/documentation/latest/endpoints/)
* [<span data-ttu-id="62bac-168">Další informace o hello rozhraní REST API Marathonu</span><span class="sxs-lookup"><span data-stu-id="62bac-168">Read more about hello Marathon REST API</span></span>](https://mesosphere.github.io/marathon/docs/rest-api.html)

