---
title: "kurz pro službu kontejneru aaaAzure – Příprava aplikace | Microsoft Docs"
description: "Kurz pro Azure Container Service – Příprava aplikace"
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, Kontejnery, mikroslužby, Kubernetes, DC/OS, Azure"
ms.assetid: 
ms.service: container-service
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: b537ecc9ff50358fb65b128bfe6eb894dd088cc4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-container-images-toobe-used-with-azure-container-service"></a><span data-ttu-id="b05cd-104">Vytvoření kontejneru toobe Image použít s Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="b05cd-104">Create container images toobe used with Azure Container Service</span></span>

<span data-ttu-id="b05cd-105">V tomto kurzu, 7, první část je více kontejnerové aplikace připravené pro použití v Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="b05cd-105">In this tutorial, part one of seven, a multi-container application is prepared for use in Kubernetes.</span></span> <span data-ttu-id="b05cd-106">Dokončit krokům patří:</span><span class="sxs-lookup"><span data-stu-id="b05cd-106">Steps completed include:</span></span>  

> [!div class="checklist"]
> * <span data-ttu-id="b05cd-107">Klonování zdroje aplikace z GitHubu</span><span class="sxs-lookup"><span data-stu-id="b05cd-107">Cloning application source from GitHub</span></span>  
> * <span data-ttu-id="b05cd-108">Vytvoření kontejneru image ze zdroje aplikace hello</span><span class="sxs-lookup"><span data-stu-id="b05cd-108">Creating a container image from hello application source</span></span>
> * <span data-ttu-id="b05cd-109">Testování aplikace hello v místním prostředí Docker</span><span class="sxs-lookup"><span data-stu-id="b05cd-109">Testing hello application in a local Docker environment</span></span>

<span data-ttu-id="b05cd-110">Po dokončení hello následující aplikace je dostupné ve vašem místním vývojovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="b05cd-110">Once completed, hello following application is accessible in your local development environment.</span></span>

![Obrázek clusteru Kubernetes v Azure](media/container-service-kubernetes-tutorials/azure-vote.png)

<span data-ttu-id="b05cd-112">V následujících kurzech hello kontejneru image je nahrané tooan registru kontejner Azure a pak spustit v Azure hostovaná Kubernetes clusteru.</span><span class="sxs-lookup"><span data-stu-id="b05cd-112">In subsequent tutorials, hello container image is uploaded tooan Azure Container Registry, and then run in an Azure hosted Kubernetes cluster.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="b05cd-113">Než začnete</span><span class="sxs-lookup"><span data-stu-id="b05cd-113">Before you begin</span></span>

<span data-ttu-id="b05cd-114">V tomto kurzu se předpokládá základní znalost klíčových konceptů Dockeru, jako jsou kontejnery, image kontejnerů a základní příkazy Dockeru.</span><span class="sxs-lookup"><span data-stu-id="b05cd-114">This tutorial assumes a basic understanding of core Docker concepts such as containers, container images, and basic docker commands.</span></span> <span data-ttu-id="b05cd-115">V případě potřeby najdete základní informace o kontejnerech v článku [Get started with Docker]( https://docs.docker.com/get-started/) (Začínáme s Dockerem).</span><span class="sxs-lookup"><span data-stu-id="b05cd-115">If needed, see [Get started with Docker]( https://docs.docker.com/get-started/) for a primer on container basics.</span></span> 

<span data-ttu-id="b05cd-116">toocomplete tohoto kurzu budete potřebovat Docker vývojovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="b05cd-116">toocomplete this tutorial, you need a Docker development environment.</span></span> <span data-ttu-id="b05cd-117">Docker nabízí balíčky pro snadnou konfiguraci Dockeru na jakémkoli systému [Mac](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) nebo [Linux](https://docs.docker.com/engine/installation/#supported-platforms).</span><span class="sxs-lookup"><span data-stu-id="b05cd-117">Docker provides packages that easily configure Docker on any [Mac](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/), or [Linux](https://docs.docker.com/engine/installation/#supported-platforms) system.</span></span>

## <a name="get-application-code"></a><span data-ttu-id="b05cd-118">Získání kódu aplikace</span><span class="sxs-lookup"><span data-stu-id="b05cd-118">Get application code</span></span>

<span data-ttu-id="b05cd-119">Ukázková aplikace Hello použili v tomto kurzu je základní hlasovací aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b05cd-119">hello sample application used in this tutorial is a basic voting app.</span></span> <span data-ttu-id="b05cd-120">Hello aplikace se skládá z front-endové webové součásti a instanci Redis back-end.</span><span class="sxs-lookup"><span data-stu-id="b05cd-120">hello application consists of a front-end web component and a back-end Redis instance.</span></span> <span data-ttu-id="b05cd-121">součást webové Hello je zabalené do bitové kopie vlastní kontejner.</span><span class="sxs-lookup"><span data-stu-id="b05cd-121">hello web component is packaged into a custom container image.</span></span> <span data-ttu-id="b05cd-122">Hello Redis instance používá image beze změny z úložiště Docker Hub.</span><span class="sxs-lookup"><span data-stu-id="b05cd-122">hello Redis instance uses an unmodified image from Docker Hub.</span></span>  

<span data-ttu-id="b05cd-123">Pomocí git toodownload kopii hello aplikace tooyour vývojové prostředí.</span><span class="sxs-lookup"><span data-stu-id="b05cd-123">Use git toodownload a copy of hello application tooyour development environment.</span></span>

```bash
git clone https://github.com/Azure-Samples/azure-voting-app-redis.git
```

<span data-ttu-id="b05cd-124">Uvnitř hello klonovaný adresář je zdrojovému kódu aplikace hello, předem vytvořené Docker compose soubor a soubor manifestu Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="b05cd-124">Inside hello cloned directory is hello application source code, a pre-created Docker compose file, and a Kubernetes manifest file.</span></span> <span data-ttu-id="b05cd-125">Tyto soubory jsou použité toocreate prostředky v rámci kurzu sadu hello.</span><span class="sxs-lookup"><span data-stu-id="b05cd-125">These files are used toocreate assets throughout hello tutorial set.</span></span> 

## <a name="create-container-images"></a><span data-ttu-id="b05cd-126">Vytvořit kontejner bitové kopie</span><span class="sxs-lookup"><span data-stu-id="b05cd-126">Create container images</span></span>

<span data-ttu-id="b05cd-127">[Docker Compose](https://docs.docker.com/compose/) lze použít sestavení hello tooautomate mimo kontejner bitové kopie a hello nasazení aplikací s více kontejneru.</span><span class="sxs-lookup"><span data-stu-id="b05cd-127">[Docker Compose](https://docs.docker.com/compose/) can be used tooautomate hello build out of container images and hello deployment of multi-container applications.</span></span>

<span data-ttu-id="b05cd-128">Spustit hello docker-compose.yml souboru toocreate hello kontejneru image, stažení hello Redis bitové kopie a spusťte aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="b05cd-128">Run hello docker-compose.yml file toocreate hello container image, download hello Redis image, and start hello application.</span></span>

```bash
docker-compose -f ./azure-voting-app-redis/docker-compose.yml up -d
```

<span data-ttu-id="b05cd-129">Po dokončení použít hello [imagí dockeru](https://docs.docker.com/engine/reference/commandline/images/) příkaz toosee hello vytvoření bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="b05cd-129">When completed, use hello [docker images](https://docs.docker.com/engine/reference/commandline/images/) command toosee hello created images.</span></span>

```bash
docker images
```

<span data-ttu-id="b05cd-130">Všimněte si, že tři bitové kopie byly staženy nebo vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="b05cd-130">Notice that three images have been downloaded or created.</span></span> <span data-ttu-id="b05cd-131">Hello *azure hlas front* image obsahuje aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="b05cd-131">hello *azure-vote-front* image contains hello application.</span></span> <span data-ttu-id="b05cd-132">Je odvozený z hello *nginx flask* bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="b05cd-132">It was derived from hello *nginx-flask* image.</span></span> <span data-ttu-id="b05cd-133">Obrázek Redis Hello byl stažen z úložiště Docker Hub.</span><span class="sxs-lookup"><span data-stu-id="b05cd-133">hello Redis image was downloaded from Docker Hub.</span></span>

```bash
REPOSITORY                   TAG        IMAGE ID            CREATED             SIZE
azure-vote-front             latest     9cc914e25834        40 seconds ago      694MB
redis                        latest     a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask   flask      788ca94b2313        9 months ago        694MB
```

<span data-ttu-id="b05cd-134">Spustit hello [docker ps](https://docs.docker.com/engine/reference/commandline/ps/) hello toosee příkaz spuštěných kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="b05cd-134">Run hello [docker ps](https://docs.docker.com/engine/reference/commandline/ps/) command toosee hello running containers.</span></span>

```bash
docker ps
```

<span data-ttu-id="b05cd-135">Výstup:</span><span class="sxs-lookup"><span data-stu-id="b05cd-135">Output:</span></span>

```bash
CONTAINER ID        IMAGE             COMMAND                  CREATED             STATUS              PORTS                           NAMES
82411933e8f9        azure-vote-front  "/usr/bin/supervisord"   57 seconds ago      Up 30 seconds       443/tcp, 0.0.0.0:8080->80/tcp   azure-vote-front
b68fed4b66b6        redis             "docker-entrypoint..."   57 seconds ago      Up 30 seconds       0.0.0.0:6379->6379/tcp          azure-vote-back
```

## <a name="test-application-locally"></a><span data-ttu-id="b05cd-136">Testovací aplikace místně</span><span class="sxs-lookup"><span data-stu-id="b05cd-136">Test application locally</span></span>

<span data-ttu-id="b05cd-137">Procházejte hello toosee toohttp://localhost:8080 spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="b05cd-137">Browse toohttp://localhost:8080 toosee hello running application.</span></span>

![Obrázek clusteru Kubernetes v Azure](media/container-service-kubernetes-tutorials/azure-vote.png)

## <a name="clean-up-resources"></a><span data-ttu-id="b05cd-139">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="b05cd-139">Clean up resources</span></span>

<span data-ttu-id="b05cd-140">Teď, když ověřila funkčnost aplikace hello spuštěných kontejnerů můžete zastavit a odebrat.</span><span class="sxs-lookup"><span data-stu-id="b05cd-140">Now that application functionality has been validated, hello running containers can be stopped and removed.</span></span> <span data-ttu-id="b05cd-141">Neodstraňujte hello kontejneru Image.</span><span class="sxs-lookup"><span data-stu-id="b05cd-141">Do not delete hello container images.</span></span> <span data-ttu-id="b05cd-142">Hello *azure hlas front* bitové kopie je instance nahrané tooan Azure kontejneru registru v dalším kurzu hello.</span><span class="sxs-lookup"><span data-stu-id="b05cd-142">hello *azure-vote-front* image is uploaded tooan Azure Container Registry instance in hello next tutorial.</span></span>

<span data-ttu-id="b05cd-143">Spusťte hello následující toostop hello spuštěných kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="b05cd-143">Run hello following toostop hello running containers.</span></span>

```bash
docker-compose -f ./azure-voting-app-redis/docker-compose.yml stop
```

<span data-ttu-id="b05cd-144">Odstranění kontejnerů hello zastavena s hello následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="b05cd-144">Delete hello stopped containers with hello following command.</span></span>

```bash
docker-compose -f ./azure-voting-app-redis/docker-compose.yml rm
```

<span data-ttu-id="b05cd-145">Při dokončení máte kontejneru bitovou kopii, která obsahuje aplikaci Azure hlas hello.</span><span class="sxs-lookup"><span data-stu-id="b05cd-145">At completion, you have a container image that contains hello Azure Vote application.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b05cd-146">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b05cd-146">Next steps</span></span>

<span data-ttu-id="b05cd-147">V tomto kurzu se testoval aplikace a bitové kopie kontejneru pro aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="b05cd-147">In this tutorial, an application was tested and container images created for hello application.</span></span> <span data-ttu-id="b05cd-148">byly dokončeny Hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b05cd-148">hello following steps were completed:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b05cd-149">Klonování zdroj aplikace hello z Githubu</span><span class="sxs-lookup"><span data-stu-id="b05cd-149">Cloning hello application source from GitHub</span></span>  
> * <span data-ttu-id="b05cd-150">Vytvořené bitové kopie kontejneru z zdroj aplikace</span><span class="sxs-lookup"><span data-stu-id="b05cd-150">Created a container image from application source</span></span>
> * <span data-ttu-id="b05cd-151">Otestované hello aplikace v místním prostředí Docker</span><span class="sxs-lookup"><span data-stu-id="b05cd-151">Tested hello application in a local Docker environment</span></span>

<span data-ttu-id="b05cd-152">Posunutí další kurz toolearn toohello o ukládání bitových kopií kontejneru v registru kontejneru služby Azure.</span><span class="sxs-lookup"><span data-stu-id="b05cd-152">Advance toohello next tutorial toolearn about storing container images in an Azure Container Registry.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="b05cd-153">Push bitové kopie tooAzure registru kontejneru</span><span class="sxs-lookup"><span data-stu-id="b05cd-153">Push images tooAzure Container Registry</span></span>](./container-service-tutorial-kubernetes-prepare-acr.md)
