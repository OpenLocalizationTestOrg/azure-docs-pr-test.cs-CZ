---
title: "Kurz pro Azure Container Service – Příprava aplikace | Microsoft Docs"
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
ms.openlocfilehash: f02ee61ef1cd3b3dfaa051cfabe52866e3e7e838
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-container-images-to-be-used-with-azure-container-service"></a><span data-ttu-id="6f450-104">Vytvoření bitové kopie kontejneru, který se má použít s Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="6f450-104">Create container images to be used with Azure Container Service</span></span>

<span data-ttu-id="6f450-105">V tomto kurzu, 7, první část je více kontejnerové aplikace připravené pro použití v Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="6f450-105">In this tutorial, part one of seven, a multi-container application is prepared for use in Kubernetes.</span></span> <span data-ttu-id="6f450-106">Dokončit krokům patří:</span><span class="sxs-lookup"><span data-stu-id="6f450-106">Steps completed include:</span></span>  

> [!div class="checklist"]
> * <span data-ttu-id="6f450-107">Klonování zdroje aplikace z GitHubu</span><span class="sxs-lookup"><span data-stu-id="6f450-107">Cloning application source from GitHub</span></span>  
> * <span data-ttu-id="6f450-108">Vytvoření kontejneru image ze zdroje aplikace</span><span class="sxs-lookup"><span data-stu-id="6f450-108">Creating a container image from the application source</span></span>
> * <span data-ttu-id="6f450-109">Testování aplikace v místním prostředí Docker</span><span class="sxs-lookup"><span data-stu-id="6f450-109">Testing the application in a local Docker environment</span></span>

<span data-ttu-id="6f450-110">Po dokončení následující aplikace je dostupné ve vašem místním vývojovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="6f450-110">Once completed, the following application is accessible in your local development environment.</span></span>

![Obrázek clusteru Kubernetes v Azure](media/container-service-kubernetes-tutorials/azure-vote.png)

<span data-ttu-id="6f450-112">V následujících kurzech bitovou kopii kontejneru se nahraje registru kontejneru služby Azure a pak spustit v Azure hostovaná Kubernetes clusteru.</span><span class="sxs-lookup"><span data-stu-id="6f450-112">In subsequent tutorials, the container image is uploaded to an Azure Container Registry, and then run in an Azure hosted Kubernetes cluster.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="6f450-113">Než začnete</span><span class="sxs-lookup"><span data-stu-id="6f450-113">Before you begin</span></span>

<span data-ttu-id="6f450-114">V tomto kurzu se předpokládá základní znalost klíčových konceptů Dockeru, jako jsou kontejnery, image kontejnerů a základní příkazy Dockeru.</span><span class="sxs-lookup"><span data-stu-id="6f450-114">This tutorial assumes a basic understanding of core Docker concepts such as containers, container images, and basic docker commands.</span></span> <span data-ttu-id="6f450-115">V případě potřeby najdete základní informace o kontejnerech v článku [Get started with Docker]( https://docs.docker.com/get-started/) (Začínáme s Dockerem).</span><span class="sxs-lookup"><span data-stu-id="6f450-115">If needed, see [Get started with Docker]( https://docs.docker.com/get-started/) for a primer on container basics.</span></span> 

<span data-ttu-id="6f450-116">K dokončení tohoto kurzu potřebujete vývojové prostředí pro Docker.</span><span class="sxs-lookup"><span data-stu-id="6f450-116">To complete this tutorial, you need a Docker development environment.</span></span> <span data-ttu-id="6f450-117">Docker nabízí balíčky pro snadnou konfiguraci Dockeru na jakémkoli systému [Mac](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) nebo [Linux](https://docs.docker.com/engine/installation/#supported-platforms).</span><span class="sxs-lookup"><span data-stu-id="6f450-117">Docker provides packages that easily configure Docker on any [Mac](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/), or [Linux](https://docs.docker.com/engine/installation/#supported-platforms) system.</span></span>

## <a name="get-application-code"></a><span data-ttu-id="6f450-118">Získání kódu aplikace</span><span class="sxs-lookup"><span data-stu-id="6f450-118">Get application code</span></span>

<span data-ttu-id="6f450-119">Ukázková aplikace používá v tomto kurzu je základní hlasovací aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6f450-119">The sample application used in this tutorial is a basic voting app.</span></span> <span data-ttu-id="6f450-120">Aplikace se skládá z front-endové webové součásti a instanci Redis back-end.</span><span class="sxs-lookup"><span data-stu-id="6f450-120">The application consists of a front-end web component and a back-end Redis instance.</span></span> <span data-ttu-id="6f450-121">Součást webové je zabalené do bitové kopie vlastní kontejner.</span><span class="sxs-lookup"><span data-stu-id="6f450-121">The web component is packaged into a custom container image.</span></span> <span data-ttu-id="6f450-122">Redis instance používá image beze změny z úložiště Docker Hub.</span><span class="sxs-lookup"><span data-stu-id="6f450-122">The Redis instance uses an unmodified image from Docker Hub.</span></span>  

<span data-ttu-id="6f450-123">Pomocí git stáhnout kopii aplikace na svoje vývojové prostředí.</span><span class="sxs-lookup"><span data-stu-id="6f450-123">Use git to download a copy of the application to your development environment.</span></span>

```bash
git clone https://github.com/Azure-Samples/azure-voting-app-redis.git
```

<span data-ttu-id="6f450-124">V adresáři klonovaný je zdrojový kód aplikace, předem vytvořené Docker compose soubor a soubor manifestu Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="6f450-124">Inside the cloned directory is the application source code, a pre-created Docker compose file, and a Kubernetes manifest file.</span></span> <span data-ttu-id="6f450-125">Tyto soubory se používají k vytvoření prostředky v celé sadě kurzu.</span><span class="sxs-lookup"><span data-stu-id="6f450-125">These files are used to create assets throughout the tutorial set.</span></span> 

## <a name="create-container-images"></a><span data-ttu-id="6f450-126">Vytvořit kontejner bitové kopie</span><span class="sxs-lookup"><span data-stu-id="6f450-126">Create container images</span></span>

<span data-ttu-id="6f450-127">[Docker Compose](https://docs.docker.com/compose/) můžete použít k automatizaci sestavení mimo kontejner bitové kopie a nasazení aplikací s více kontejneru.</span><span class="sxs-lookup"><span data-stu-id="6f450-127">[Docker Compose](https://docs.docker.com/compose/) can be used to automate the build out of container images and the deployment of multi-container applications.</span></span>

<span data-ttu-id="6f450-128">Spusťte soubor docker-compose.yml na vytvoření bitové kopie kontejneru, stáhněte bitovou kopii Redis a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6f450-128">Run the docker-compose.yml file to create the container image, download the Redis image, and start the application.</span></span>

```bash
docker-compose -f ./azure-voting-app-redis/docker-compose.yml up -d
```

<span data-ttu-id="6f450-129">Po dokončení použít [imagí dockeru](https://docs.docker.com/engine/reference/commandline/images/) příkazu zobrazte vytvořené bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="6f450-129">When completed, use the [docker images](https://docs.docker.com/engine/reference/commandline/images/) command to see the created images.</span></span>

```bash
docker images
```

<span data-ttu-id="6f450-130">Všimněte si, že tři bitové kopie byly staženy nebo vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="6f450-130">Notice that three images have been downloaded or created.</span></span> <span data-ttu-id="6f450-131">*Azure hlas front* image obsahuje aplikace.</span><span class="sxs-lookup"><span data-stu-id="6f450-131">The *azure-vote-front* image contains the application.</span></span> <span data-ttu-id="6f450-132">Byl odebrán z *nginx flask* bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="6f450-132">It was derived from the *nginx-flask* image.</span></span> <span data-ttu-id="6f450-133">Bitovou kopii Redis byl stažen z úložiště Docker Hub.</span><span class="sxs-lookup"><span data-stu-id="6f450-133">The Redis image was downloaded from Docker Hub.</span></span>

```bash
REPOSITORY                   TAG        IMAGE ID            CREATED             SIZE
azure-vote-front             latest     9cc914e25834        40 seconds ago      694MB
redis                        latest     a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask   flask      788ca94b2313        9 months ago        694MB
```

<span data-ttu-id="6f450-134">Spustit [docker ps](https://docs.docker.com/engine/reference/commandline/ps/) příkazu zobrazte spuštěných kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="6f450-134">Run the [docker ps](https://docs.docker.com/engine/reference/commandline/ps/) command to see the running containers.</span></span>

```bash
docker ps
```

<span data-ttu-id="6f450-135">Výstup:</span><span class="sxs-lookup"><span data-stu-id="6f450-135">Output:</span></span>

```bash
CONTAINER ID        IMAGE             COMMAND                  CREATED             STATUS              PORTS                           NAMES
82411933e8f9        azure-vote-front  "/usr/bin/supervisord"   57 seconds ago      Up 30 seconds       443/tcp, 0.0.0.0:8080->80/tcp   azure-vote-front
b68fed4b66b6        redis             "docker-entrypoint..."   57 seconds ago      Up 30 seconds       0.0.0.0:6379->6379/tcp          azure-vote-back
```

## <a name="test-application-locally"></a><span data-ttu-id="6f450-136">Testovací aplikace místně</span><span class="sxs-lookup"><span data-stu-id="6f450-136">Test application locally</span></span>

<span data-ttu-id="6f450-137">Přejděte na adrese http://localhost: 8080 zobrazíte běžící aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6f450-137">Browse to http://localhost:8080 to see the running application.</span></span>

![Obrázek clusteru Kubernetes v Azure](media/container-service-kubernetes-tutorials/azure-vote.png)

## <a name="clean-up-resources"></a><span data-ttu-id="6f450-139">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="6f450-139">Clean up resources</span></span>

<span data-ttu-id="6f450-140">Teď, když funkce aplikace byl ověřen, může spuštěných kontejnerů zastavena a odebrat.</span><span class="sxs-lookup"><span data-stu-id="6f450-140">Now that application functionality has been validated, the running containers can be stopped and removed.</span></span> <span data-ttu-id="6f450-141">Neodstraňujte kontejneru bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="6f450-141">Do not delete the container images.</span></span> <span data-ttu-id="6f450-142">*Azure hlas front* bitové kopie se nahraje instanci Azure Container registru v dalším kurzu.</span><span class="sxs-lookup"><span data-stu-id="6f450-142">The *azure-vote-front* image is uploaded to an Azure Container Registry instance in the next tutorial.</span></span>

<span data-ttu-id="6f450-143">Spusťte následující příkaz k zastavení spuštěných kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="6f450-143">Run the following to stop the running containers.</span></span>

```bash
docker-compose -f ./azure-voting-app-redis/docker-compose.yml stop
```

<span data-ttu-id="6f450-144">Odstranění zastaven kontejnerů pomocí následujícího příkazu.</span><span class="sxs-lookup"><span data-stu-id="6f450-144">Delete the stopped containers with the following command.</span></span>

```bash
docker-compose -f ./azure-voting-app-redis/docker-compose.yml rm
```

<span data-ttu-id="6f450-145">Při dokončení máte kontejneru bitovou kopii, která obsahuje aplikaci Azure hlas.</span><span class="sxs-lookup"><span data-stu-id="6f450-145">At completion, you have a container image that contains the Azure Vote application.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6f450-146">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6f450-146">Next steps</span></span>

<span data-ttu-id="6f450-147">V tomto kurzu aplikace byla testována a vytvořit kontejner bitových kopií pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6f450-147">In this tutorial, an application was tested and container images created for the application.</span></span> <span data-ttu-id="6f450-148">Dokončili jste následující kroky:</span><span class="sxs-lookup"><span data-stu-id="6f450-148">The following steps were completed:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="6f450-149">Klonování zdroje aplikace z GitHubu</span><span class="sxs-lookup"><span data-stu-id="6f450-149">Cloning the application source from GitHub</span></span>  
> * <span data-ttu-id="6f450-150">Vytvořené bitové kopie kontejneru z zdroj aplikace</span><span class="sxs-lookup"><span data-stu-id="6f450-150">Created a container image from application source</span></span>
> * <span data-ttu-id="6f450-151">Testování aplikace v místním prostředí Docker</span><span class="sxs-lookup"><span data-stu-id="6f450-151">Tested the application in a local Docker environment</span></span>

<span data-ttu-id="6f450-152">Přejděte k dalšímu kurzu, ve kterém se seznámíte s ukládáním imagí kontejnerů ve službě Azure Container Registry.</span><span class="sxs-lookup"><span data-stu-id="6f450-152">Advance to the next tutorial to learn about storing container images in an Azure Container Registry.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="6f450-153">Nahrávání imagí do služby Azure Container Registry</span><span class="sxs-lookup"><span data-stu-id="6f450-153">Push images to Azure Container Registry</span></span>](./container-service-tutorial-kubernetes-prepare-acr.md)