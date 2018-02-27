---
title: "Správě clusteru Swarm v Azure s rozhraním API Docker"
description: "Nasazení kontejnerů do clusteru s podporou Docker Swarm v Azure Container Service"
services: container-service
author: rgardler
manager: madhana
ms.service: container-service
ms.topic: article
ms.date: 09/13/2016
ms.author: rogardle
ms.custom: mvc
ms.openlocfilehash: 3f8d18bc053bc303ab124ba38c8621d4ee2e8cb8
ms.sourcegitcommit: 694e40a193980dea1e2f945471071f11030d5641
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
# <a name="container-management-with-docker-swarm"></a><span data-ttu-id="4ccaf-103">Správa kontejnerů pomocí nástroje Docker Swarm</span><span class="sxs-lookup"><span data-stu-id="4ccaf-103">Container management with Docker Swarm</span></span>

<span data-ttu-id="4ccaf-104">Docker Swarm poskytuje prostředí pro nasazování kontejnerizovaných úloh v celé sadě hostitelů Docker uspořádaných do fondu.</span><span class="sxs-lookup"><span data-stu-id="4ccaf-104">Docker Swarm provides an environment for deploying containerized workloads across a pooled set of Docker hosts.</span></span> <span data-ttu-id="4ccaf-105">Docker Swarm využívá nativní rozhraní Docker API.</span><span class="sxs-lookup"><span data-stu-id="4ccaf-105">Docker Swarm uses the native Docker API.</span></span> <span data-ttu-id="4ccaf-106">Workflow správy kontejnerů v nástroji Docker Swarm je téměř identický s workflow pro hostitele s jedním kontejnerem.</span><span class="sxs-lookup"><span data-stu-id="4ccaf-106">The workflow for managing containers on a Docker Swarm is almost identical to what it would be on a single container host.</span></span> <span data-ttu-id="4ccaf-107">Tento dokument poskytuje jednoduché příklady, jak nasadit kontejnerizované úlohy do instance Docker Swarm v Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="4ccaf-107">This document provides simple examples of deploying containerized workloads in an Azure Container Service instance of Docker Swarm.</span></span> <span data-ttu-id="4ccaf-108">Podrobnější dokumentaci k nástroji Docker Swarm najdete v tématu [Docker Swarm na Docker.com](https://docs.docker.com/swarm/).</span><span class="sxs-lookup"><span data-stu-id="4ccaf-108">For more in-depth documentation on Docker Swarm, see [Docker Swarm on Docker.com](https://docs.docker.com/swarm/).</span></span>

[!INCLUDE [container-service-swarm-mode-note](../../../includes/container-service-swarm-mode-note.md)]

<span data-ttu-id="4ccaf-109">Předpoklady pro praktická cvičení v tomto dokumentu:</span><span class="sxs-lookup"><span data-stu-id="4ccaf-109">Prerequisites to the exercises in this document:</span></span>

[<span data-ttu-id="4ccaf-110">Vytvoření clusteru Swarm v Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="4ccaf-110">Create a Swarm cluster in Azure Container Service</span></span>](container-service-deployment.md)

[<span data-ttu-id="4ccaf-111">Propojení s clusterem Swarm ve službě Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="4ccaf-111">Connect with the Swarm cluster in Azure Container Service</span></span>](../container-service-connect.md)

## <a name="deploy-a-new-container"></a><span data-ttu-id="4ccaf-112">Nasazení nového kontejneru</span><span class="sxs-lookup"><span data-stu-id="4ccaf-112">Deploy a new container</span></span>
<span data-ttu-id="4ccaf-113">Chcete-li vytvořit nový kontejner v Docker Swarm, použijte příkaz `docker run` (zajistí, že jste otevřeli tunelové propojení SSH k předlohám podle výše uvedených požadavků).</span><span class="sxs-lookup"><span data-stu-id="4ccaf-113">To create a new container in the Docker Swarm, use the `docker run` command (ensuring that you have opened an SSH tunnel to the masters as per the prerequisites above).</span></span> <span data-ttu-id="4ccaf-114">Tento příklad vytvoří kontejner z image `yeasy/simple-web`:</span><span class="sxs-lookup"><span data-stu-id="4ccaf-114">This example creates a container from the `yeasy/simple-web` image:</span></span>

```bash
user@ubuntu:~$ docker run -d -p 80:80 yeasy/simple-web

4298d397b9ab6f37e2d1978ef3c8c1537c938e98a8bf096ff00def2eab04bf72
```

<span data-ttu-id="4ccaf-115">Až se kontejner vytvoří, zobrazte si informace o kontejneru příkazem `docker ps`.</span><span class="sxs-lookup"><span data-stu-id="4ccaf-115">After the container has been created, use `docker ps` to return information about the container.</span></span> <span data-ttu-id="4ccaf-116">Můžete si zde všimnout, že je uveden agent Swarm, který je hostitelem kontejneru:</span><span class="sxs-lookup"><span data-stu-id="4ccaf-116">Notice here that the Swarm agent that is hosting the container is listed:</span></span>

```bash
user@ubuntu:~$ docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                 NAMES
4298d397b9ab        yeasy/simple-web    "/bin/sh -c 'python i"   31 seconds ago      Up 9 seconds        10.0.0.5:80->80/tcp   swarm-agent-34A73819-1/happy_allen
```  

<span data-ttu-id="4ccaf-117">Nyní můžete k aplikaci, která běží v tomto kontejneru, přistoupit přes veřejný název DNS nástroje pro vyrovnávání zatížení agenta Swarm.</span><span class="sxs-lookup"><span data-stu-id="4ccaf-117">You can now access the application that is running in this container through the public DNS name of the Swarm agent load balancer.</span></span> <span data-ttu-id="4ccaf-118">Na Portálu Azure lze najít tyto informace:</span><span class="sxs-lookup"><span data-stu-id="4ccaf-118">You can find this information in the Azure portal:</span></span>  

![Výsledky skutečných návštěv](./media/container-service-docker-swarm/real-visit.jpg)  

<span data-ttu-id="4ccaf-120">Ve výchozím nastavení má nástroj pro vyrovnávání zatížení otevřené porty 80, 8080 a 443.</span><span class="sxs-lookup"><span data-stu-id="4ccaf-120">By default the Load Balancer has ports 80, 8080 and 443 open.</span></span> <span data-ttu-id="4ccaf-121">Pokud se chcete připojit k jinému portu, musíte otevřít tento port v nástroji Azure Load Balancer pro fond agenta.</span><span class="sxs-lookup"><span data-stu-id="4ccaf-121">If you want to connect on another port you will need to open that port on the Azure Load Balancer for the Agent Pool.</span></span>

## <a name="deploy-multiple-containers"></a><span data-ttu-id="4ccaf-122">Nasazení několika kontejnerů</span><span class="sxs-lookup"><span data-stu-id="4ccaf-122">Deploy multiple containers</span></span>
<span data-ttu-id="4ccaf-123">Když je spuštěno více kontejnerů, je možné vícenásobným spuštěním příkazu „spuštění docker“ použít příkaz `docker ps` k zobrazení, na kterých hostitelích kontejnery běží.</span><span class="sxs-lookup"><span data-stu-id="4ccaf-123">As multiple containers are started, by executing 'docker run' multiple times, you can use the `docker ps` command to see which hosts the containers are running on.</span></span> <span data-ttu-id="4ccaf-124">V tomto příkladu níže jsou tři kontejnery rovnoměrně rozloženy na tři agenty Swarm:</span><span class="sxs-lookup"><span data-stu-id="4ccaf-124">In the example below, three containers are spread evenly across the three Swarm agents:</span></span>  

```bash
user@ubuntu:~$ docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                 NAMES
11be062ff602        yeasy/simple-web    "/bin/sh -c 'python i"   11 seconds ago      Up 10 seconds       10.0.0.6:83->80/tcp   swarm-agent-34A73819-2/clever_banach
1ff421554c50        yeasy/simple-web    "/bin/sh -c 'python i"   49 seconds ago      Up 48 seconds       10.0.0.4:82->80/tcp   swarm-agent-34A73819-0/stupefied_ride
4298d397b9ab        yeasy/simple-web    "/bin/sh -c 'python i"   2 minutes ago       Up 2 minutes        10.0.0.5:80->80/tcp   swarm-agent-34A73819-1/happy_allen
```  

## <a name="deploy-containers-by-using-docker-compose"></a><span data-ttu-id="4ccaf-125">Nasazení kontejnerů pomocí Docker Compose</span><span class="sxs-lookup"><span data-stu-id="4ccaf-125">Deploy containers by using Docker Compose</span></span>
<span data-ttu-id="4ccaf-126">Pomocí Docker Compose je možné automatizovat nasazení a konfiguraci několika kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="4ccaf-126">You can use Docker Compose to automate the deployment and configuration of multiple containers.</span></span> <span data-ttu-id="4ccaf-127">Abyste toho mohli využít, ujistěte se, že je vytvořen tunel Secure Shell (SSH) a že je nastavena proměnná DOCKER_HOST (viz předpoklady výše).</span><span class="sxs-lookup"><span data-stu-id="4ccaf-127">To do so, ensure that a Secure Shell (SSH) tunnel has been created and that the DOCKER_HOST variable has been set (see the pre-requisites above).</span></span>

<span data-ttu-id="4ccaf-128">Vytvořte v lokálním systému soubor docker-compose.yml.</span><span class="sxs-lookup"><span data-stu-id="4ccaf-128">Create a docker-compose.yml file on your local system.</span></span> <span data-ttu-id="4ccaf-129">Použijte k tomu tuto [ukázku](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/docker-compose.yml).</span><span class="sxs-lookup"><span data-stu-id="4ccaf-129">To do this, use this [sample](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/docker-compose.yml).</span></span>

```bash
web:
  image: adtd/web:0.1
  ports:
    - "80:80"
  links:
    - rest:rest-demo-azure.marathon.mesos
rest:
  image: adtd/rest:0.1
  ports:
    - "8080:8080"

```

<span data-ttu-id="4ccaf-130">Příkazem `docker-compose up -d` spusťte nasazení kontejnerů:</span><span class="sxs-lookup"><span data-stu-id="4ccaf-130">Run `docker-compose up -d` to start the container deployments:</span></span>

```bash
user@ubuntu:~/compose$ docker-compose up -d
Pulling rest (adtd/rest:0.1)...
swarm-agent-3B7093B8-0: Pulling adtd/rest:0.1... : downloaded
swarm-agent-3B7093B8-2: Pulling adtd/rest:0.1... : downloaded
swarm-agent-3B7093B8-3: Pulling adtd/rest:0.1... : downloaded
Creating compose_rest_1
Pulling web (adtd/web:0.1)...
swarm-agent-3B7093B8-3: Pulling adtd/web:0.1... : downloaded
swarm-agent-3B7093B8-0: Pulling adtd/web:0.1... : downloaded
swarm-agent-3B7093B8-2: Pulling adtd/web:0.1... : downloaded
Creating compose_web_1
```

<span data-ttu-id="4ccaf-131">Nakonec se vrátí seznam spuštěných kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="4ccaf-131">Finally, the list of running containers will be returned.</span></span> <span data-ttu-id="4ccaf-132">Tento seznam reflektuje kontejnery nasazené pomocí Docker Compose:</span><span class="sxs-lookup"><span data-stu-id="4ccaf-132">This list reflects the containers that were deployed by using Docker Compose:</span></span>

```bash
user@ubuntu:~/compose$ docker ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS                     NAMES
caf185d221b7        adtd/web:0.1        "apache2-foreground"   2 minutes ago       Up About a minute   10.0.0.4:80->80/tcp       swarm-agent-3B7093B8-0/compose_web_1
040efc0ea937        adtd/rest:0.1       "catalina.sh run"      3 minutes ago       Up 2 minutes        10.0.0.4:8080->8080/tcp   swarm-agent-3B7093B8-0/compose_rest_1
```

<span data-ttu-id="4ccaf-133">Přirozeně, můžete použít `docker-compose ps` k prozkoumání samotných kontejnerů definovaných ve vašem souboru `compose.yml`.</span><span class="sxs-lookup"><span data-stu-id="4ccaf-133">Naturally, you can use `docker-compose ps` to examine only the containers defined in your `compose.yml` file.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4ccaf-134">Další postup</span><span class="sxs-lookup"><span data-stu-id="4ccaf-134">Next steps</span></span>
[<span data-ttu-id="4ccaf-135">Další informace o Docker Swarmu</span><span class="sxs-lookup"><span data-stu-id="4ccaf-135">Learn more about Docker Swarm</span></span>](https://docs.docker.com/swarm/)

