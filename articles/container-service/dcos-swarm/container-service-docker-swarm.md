---
title: "aaaManage Azure Swarm clusteru s rozhraním API Docker | Microsoft Docs"
description: "Nasazení clusteru Docker Swarm kontejnery tooa v Azure Container Service"
services: container-service
documentationcenter: 
author: rgardler
manager: madhana
editor: 
tags: acs, azure-container-service
keywords: "Docker, kontejnery, mikroslužby, Mesos, Azure"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/13/2016
ms.author: rogardle
ms.custom: mvc
ms.openlocfilehash: bb9b07c82a7b48caeb2e351455797cbf2a6e7480
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="container-management-with-docker-swarm"></a><span data-ttu-id="e5016-104">Správa kontejnerů pomocí nástroje Docker Swarm</span><span class="sxs-lookup"><span data-stu-id="e5016-104">Container management with Docker Swarm</span></span>
<span data-ttu-id="e5016-105">Docker Swarm poskytuje prostředí pro nasazování kontejnerizovaných úloh v celé sadě hostitelů Docker uspořádaných do fondu.</span><span class="sxs-lookup"><span data-stu-id="e5016-105">Docker Swarm provides an environment for deploying containerized workloads across a pooled set of Docker hosts.</span></span> <span data-ttu-id="e5016-106">Docker Swarm používá hello nativní Docker API.</span><span class="sxs-lookup"><span data-stu-id="e5016-106">Docker Swarm uses hello native Docker API.</span></span> <span data-ttu-id="e5016-107">Hello workflow správy kontejnerů v nástroji Docker Swarm je téměř identický toowhat, které má být na hostitele s jedním kontejnerem.</span><span class="sxs-lookup"><span data-stu-id="e5016-107">hello workflow for managing containers on a Docker Swarm is almost identical toowhat it would be on a single container host.</span></span> <span data-ttu-id="e5016-108">Tento dokument poskytuje jednoduché příklady, jak nasadit kontejnerizované úlohy do instance Docker Swarm v Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="e5016-108">This document provides simple examples of deploying containerized workloads in an Azure Container Service instance of Docker Swarm.</span></span> <span data-ttu-id="e5016-109">Podrobnější dokumentaci k nástroji Docker Swarm najdete v tématu [Docker Swarm na Docker.com](https://docs.docker.com/swarm/).</span><span class="sxs-lookup"><span data-stu-id="e5016-109">For more in-depth documentation on Docker Swarm, see [Docker Swarm on Docker.com](https://docs.docker.com/swarm/).</span></span>

[!INCLUDE [container-service-swarm-mode-note](../../../includes/container-service-swarm-mode-note.md)]

<span data-ttu-id="e5016-110">Cvičení toohello požadavky v tomto dokumentu:</span><span class="sxs-lookup"><span data-stu-id="e5016-110">Prerequisites toohello exercises in this document:</span></span>

[<span data-ttu-id="e5016-111">Vytvoření clusteru Swarm v Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="e5016-111">Create a Swarm cluster in Azure Container Service</span></span>](container-service-deployment.md)

[<span data-ttu-id="e5016-112">Propojení s clusterem Swarm hello v Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="e5016-112">Connect with hello Swarm cluster in Azure Container Service</span></span>](../container-service-connect.md)

## <a name="deploy-a-new-container"></a><span data-ttu-id="e5016-113">Nasazení nového kontejneru</span><span class="sxs-lookup"><span data-stu-id="e5016-113">Deploy a new container</span></span>
<span data-ttu-id="e5016-114">toocreate nový kontejner v hello Docker Swarm, použijte hello `docker run` příkazu (zajistíte, že máte otevřený SSH tunel toohello hlavních serverů podle výš uvedené požadavky hello).</span><span class="sxs-lookup"><span data-stu-id="e5016-114">toocreate a new container in hello Docker Swarm, use hello `docker run` command (ensuring that you have opened an SSH tunnel toohello masters as per hello prerequisites above).</span></span> <span data-ttu-id="e5016-115">Tento příklad vytvoří kontejner z hello `yeasy/simple-web` bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="e5016-115">This example creates a container from hello `yeasy/simple-web` image:</span></span>

```bash
user@ubuntu:~$ docker run -d -p 80:80 yeasy/simple-web

4298d397b9ab6f37e2d1978ef3c8c1537c938e98a8bf096ff00def2eab04bf72
```

<span data-ttu-id="e5016-116">Po vytvoření kontejneru hello použít `docker ps` tooreturn informace o kontejneru hello.</span><span class="sxs-lookup"><span data-stu-id="e5016-116">After hello container has been created, use `docker ps` tooreturn information about hello container.</span></span> <span data-ttu-id="e5016-117">Můžete si zde všimněte, že je uvedený tohoto agenta Swarm hello, který je hostitelem kontejneru hello:</span><span class="sxs-lookup"><span data-stu-id="e5016-117">Notice here that hello Swarm agent that is hosting hello container is listed:</span></span>

```bash
user@ubuntu:~$ docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                 NAMES
4298d397b9ab        yeasy/simple-web    "/bin/sh -c 'python i"   31 seconds ago      Up 9 seconds        10.0.0.5:80->80/tcp   swarm-agent-34A73819-1/happy_allen
```  

<span data-ttu-id="e5016-118">Nyní máte přístup hello aplikace, která běží v tomto kontejneru prostřednictvím hello veřejný název DNS služby Vyrovnávání zatížení agenta Swarm hello.</span><span class="sxs-lookup"><span data-stu-id="e5016-118">You can now access hello application that is running in this container through hello public DNS name of hello Swarm agent load balancer.</span></span> <span data-ttu-id="e5016-119">Tyto informace můžete najít v hello portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="e5016-119">You can find this information in hello Azure portal:</span></span>  

![Výsledky skutečných návštěv](./media/container-service-docker-swarm/real-visit.jpg)  

<span data-ttu-id="e5016-121">Ve výchozím nastavení má hello nástroj pro vyrovnávání zatížení porty 80, 8080 a 443 otevřete.</span><span class="sxs-lookup"><span data-stu-id="e5016-121">By default hello Load Balancer has ports 80, 8080 and 443 open.</span></span> <span data-ttu-id="e5016-122">Pokud chcete, aby tooconnect na jiný port musíte tooopen tento port na hello nástroj pro vyrovnávání zatížení Azure pro hello fondu agenta.</span><span class="sxs-lookup"><span data-stu-id="e5016-122">If you want tooconnect on another port you will need tooopen that port on hello Azure Load Balancer for hello Agent Pool.</span></span>

## <a name="deploy-multiple-containers"></a><span data-ttu-id="e5016-123">Nasazení několika kontejnerů</span><span class="sxs-lookup"><span data-stu-id="e5016-123">Deploy multiple containers</span></span>
<span data-ttu-id="e5016-124">Jak je spuštěno více kontejnerů, spuštěním 'docker spustit' vícekrát, můžete použít hello `docker ps` toosee příkaz, který hostuje hello kontejnery běží.</span><span class="sxs-lookup"><span data-stu-id="e5016-124">As multiple containers are started, by executing 'docker run' multiple times, you can use hello `docker ps` command toosee which hosts hello containers are running on.</span></span> <span data-ttu-id="e5016-125">V příkladu hello níže jsou tři kontejnery rovnoměrně rozloženy hello tři agenty Swarm:</span><span class="sxs-lookup"><span data-stu-id="e5016-125">In hello example below, three containers are spread evenly across hello three Swarm agents:</span></span>  

```bash
user@ubuntu:~$ docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                 NAMES
11be062ff602        yeasy/simple-web    "/bin/sh -c 'python i"   11 seconds ago      Up 10 seconds       10.0.0.6:83->80/tcp   swarm-agent-34A73819-2/clever_banach
1ff421554c50        yeasy/simple-web    "/bin/sh -c 'python i"   49 seconds ago      Up 48 seconds       10.0.0.4:82->80/tcp   swarm-agent-34A73819-0/stupefied_ride
4298d397b9ab        yeasy/simple-web    "/bin/sh -c 'python i"   2 minutes ago       Up 2 minutes        10.0.0.5:80->80/tcp   swarm-agent-34A73819-1/happy_allen
```  

## <a name="deploy-containers-by-using-docker-compose"></a><span data-ttu-id="e5016-126">Nasazení kontejnerů pomocí Docker Compose</span><span class="sxs-lookup"><span data-stu-id="e5016-126">Deploy containers by using Docker Compose</span></span>
<span data-ttu-id="e5016-127">Můžete použít Docker Compose tooautomate hello nasazení a konfiguraci několika kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="e5016-127">You can use Docker Compose tooautomate hello deployment and configuration of multiple containers.</span></span> <span data-ttu-id="e5016-128">toodo Ano, zkontrolujte, zda je vytvořen tunel Secure Shell (SSH) a byla nastavena proměnná DOCKER_HOST této hello (viz hello požadavky výše).</span><span class="sxs-lookup"><span data-stu-id="e5016-128">toodo so, ensure that a Secure Shell (SSH) tunnel has been created and that hello DOCKER_HOST variable has been set (see hello pre-requisites above).</span></span>

<span data-ttu-id="e5016-129">Vytvořte v lokálním systému soubor docker-compose.yml.</span><span class="sxs-lookup"><span data-stu-id="e5016-129">Create a docker-compose.yml file on your local system.</span></span> <span data-ttu-id="e5016-130">toodo, použijte ji [ukázka](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/docker-compose.yml).</span><span class="sxs-lookup"><span data-stu-id="e5016-130">toodo this, use this [sample](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/docker-compose.yml).</span></span>

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

<span data-ttu-id="e5016-131">Spusťte `docker-compose up -d` nasazení kontejnerů toostart hello:</span><span class="sxs-lookup"><span data-stu-id="e5016-131">Run `docker-compose up -d` toostart hello container deployments:</span></span>

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

<span data-ttu-id="e5016-132">Nakonec se vrátí hello seznam spuštěných kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="e5016-132">Finally, hello list of running containers will be returned.</span></span> <span data-ttu-id="e5016-133">Tento seznam obsahuje hello kontejnery, které byly nasazeny pomocí Docker Compose:</span><span class="sxs-lookup"><span data-stu-id="e5016-133">This list reflects hello containers that were deployed by using Docker Compose:</span></span>

```bash
user@ubuntu:~/compose$ docker ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS                     NAMES
caf185d221b7        adtd/web:0.1        "apache2-foreground"   2 minutes ago       Up About a minute   10.0.0.4:80->80/tcp       swarm-agent-3B7093B8-0/compose_web_1
040efc0ea937        adtd/rest:0.1       "catalina.sh run"      3 minutes ago       Up 2 minutes        10.0.0.4:8080->8080/tcp   swarm-agent-3B7093B8-0/compose_rest_1
```

<span data-ttu-id="e5016-134">Samozřejmě můžete použít `docker-compose ps` tooexamine pouze hello kontejnery, které jsou definované v vaší `compose.yml` souboru.</span><span class="sxs-lookup"><span data-stu-id="e5016-134">Naturally, you can use `docker-compose ps` tooexamine only hello containers defined in your `compose.yml` file.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e5016-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e5016-135">Next steps</span></span>
[<span data-ttu-id="e5016-136">Další informace o Docker Swarmu</span><span class="sxs-lookup"><span data-stu-id="e5016-136">Learn more about Docker Swarm</span></span>](https://docs.docker.com/swarm/)

