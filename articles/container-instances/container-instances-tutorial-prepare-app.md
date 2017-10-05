---
title: "Kurz služby Azure Container Instances – Příprava aplikace | Dokumentace Azure"
description: "Příprava aplikace pro nasazení do služby Azure Container Instances"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/01/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 167297e10eed11833623ff797e676ad43c65f9ad
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-container-for-deployment-to-azure-container-instances"></a><span data-ttu-id="33425-103">Vytvoření kontejneru pro nasazení do služby Azure Container Instances</span><span class="sxs-lookup"><span data-stu-id="33425-103">Create container for deployment to Azure Container Instances</span></span>

<span data-ttu-id="33425-104">Azure Container Instances umožňuje nasazení kontejnerů Dockeru na infrastrukturu Azure bez zřizování virtuálních počítačů nebo využívání služby vyšší úrovně.</span><span class="sxs-lookup"><span data-stu-id="33425-104">Azure Container Instances enables deployment of Docker containers onto Azure infrastructure without provisioning any virtual machines or adopting any higher-level service.</span></span> <span data-ttu-id="33425-105">V tomto kurzu vytvoříte jednoduchou webovou aplikaci v Node.js a zabalíte ji do kontejneru, který bude možné spustit pomocí služby Azure Container Instances.</span><span class="sxs-lookup"><span data-stu-id="33425-105">In this tutorial, you will build a simple web application in Node.js and package it in a container that can be run using Azure Container Instances.</span></span> <span data-ttu-id="33425-106">Společně probereme:</span><span class="sxs-lookup"><span data-stu-id="33425-106">We will cover:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="33425-107">Klonování zdroje aplikace z GitHubu</span><span class="sxs-lookup"><span data-stu-id="33425-107">Cloning application source from GitHub</span></span>  
> * <span data-ttu-id="33425-108">Vytváření imagí kontejnerů ze zdroje aplikace</span><span class="sxs-lookup"><span data-stu-id="33425-108">Creating container images from application source</span></span>
> * <span data-ttu-id="33425-109">Testování imagí v místním prostředí Dockeru</span><span class="sxs-lookup"><span data-stu-id="33425-109">Testing the images in a local Docker environment</span></span>

<span data-ttu-id="33425-110">V dalších kurzech nahrajete svou image do služby Azure Container Registry a potom ji nasadíte do služby Azure Container Instances.</span><span class="sxs-lookup"><span data-stu-id="33425-110">In subsequent tutorials, you will upload your image to an Azure Container Registry, and then deploy them to Azure Container Instances.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="33425-111">Než začnete</span><span class="sxs-lookup"><span data-stu-id="33425-111">Before you begin</span></span>

<span data-ttu-id="33425-112">V tomto kurzu se předpokládá základní znalost klíčových konceptů Dockeru, jako jsou kontejnery, image kontejnerů a základní příkazy Dockeru.</span><span class="sxs-lookup"><span data-stu-id="33425-112">This tutorial assumes a basic understanding of core Docker concepts such as containers, container images, and basic docker commands.</span></span> <span data-ttu-id="33425-113">V případě potřeby najdete základní informace o kontejnerech v článku [Get started with Docker]( https://docs.docker.com/get-started/) (Začínáme s Dockerem).</span><span class="sxs-lookup"><span data-stu-id="33425-113">If needed, see [Get started with Docker]( https://docs.docker.com/get-started/) for a primer on container basics.</span></span> 

<span data-ttu-id="33425-114">K dokončení tohoto kurzu potřebujete vývojové prostředí pro Docker.</span><span class="sxs-lookup"><span data-stu-id="33425-114">To complete this tutorial, you need a Docker development environment.</span></span> <span data-ttu-id="33425-115">Docker nabízí balíčky pro snadnou konfiguraci Dockeru na jakémkoli systému [Mac](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) nebo [Linux](https://docs.docker.com/engine/installation/#supported-platforms).</span><span class="sxs-lookup"><span data-stu-id="33425-115">Docker provides packages that easily configure Docker on any [Mac](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/), or [Linux](https://docs.docker.com/engine/installation/#supported-platforms) system.</span></span>

## <a name="get-application-code"></a><span data-ttu-id="33425-116">Získání kódu aplikace</span><span class="sxs-lookup"><span data-stu-id="33425-116">Get application code</span></span>

<span data-ttu-id="33425-117">Ukázka v tomto kurzu zahrnuje jednoduchou webovou aplikaci vytvořenou v [Node.js](http://nodejs.org).</span><span class="sxs-lookup"><span data-stu-id="33425-117">The sample in this tutorial includes a simple web application built in [Node.js](http://nodejs.org).</span></span> <span data-ttu-id="33425-118">Tato aplikace slouží jako statická stránka HTML a vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="33425-118">The app serves a static HTML page and looks like this:</span></span>

![Ukázková aplikace zobrazená v prohlížeči][aci-tutorial-app]

<span data-ttu-id="33425-120">Pomocí gitu si stáhněte ukázku:</span><span class="sxs-lookup"><span data-stu-id="33425-120">Use git to download the sample:</span></span>

```bash
git clone https://github.com/Azure-Samples/aci-helloworld.git
```

## <a name="build-the-container-image"></a><span data-ttu-id="33425-121">Sestavení image kontejneru</span><span class="sxs-lookup"><span data-stu-id="33425-121">Build the container image</span></span>

<span data-ttu-id="33425-122">Soubor Dockerfile, který je součástí ukázkového úložiště, ukazuje postup sestavení kontejneru.</span><span class="sxs-lookup"><span data-stu-id="33425-122">The Dockerfile provided in the sample repo shows how the container is built.</span></span> <span data-ttu-id="33425-123">Začíná od [oficiální image Node.js][dockerhub-nodeimage] založené na systému [Alpine Linux](https://alpinelinux.org/), malé distribuci vhodné pro použití s kontejnery.</span><span class="sxs-lookup"><span data-stu-id="33425-123">It starts from an [official Node.js image][dockerhub-nodeimage] based on [Alpine Linux](https://alpinelinux.org/), a small distribution that is well suited to use with containers.</span></span> <span data-ttu-id="33425-124">Potom zkopíruje soubory aplikace do kontejneru, nainstaluje závislosti pomocí Node Package Manageru a nakonec aplikaci spustí.</span><span class="sxs-lookup"><span data-stu-id="33425-124">It then copies the application files into the container, installs dependencies using the Node Package Manager, and finally starts the application.</span></span>

```
FROM node:8.2.0-alpine
RUN mkdir -p /usr/src/app
COPY ./app/* /usr/src/app/
WORKDIR /usr/src/app
RUN npm install
CMD node /usr/src/app/index.js
```

<span data-ttu-id="33425-125">Pomocí příkazu `docker build` vytvořte image kontejneru a označte ji jako *aci-tutorial-app*:</span><span class="sxs-lookup"><span data-stu-id="33425-125">Use the `docker build` command to create the container image, tagging it as *aci-tutorial-app*:</span></span>

```bash
docker build ./aci-helloworld -t aci-tutorial-app
```

<span data-ttu-id="33425-126">Pomocí příkazu `docker images` zobrazte sestavenou image:</span><span class="sxs-lookup"><span data-stu-id="33425-126">Use the `docker images` to see the built image:</span></span>

```bash
docker images
```

<span data-ttu-id="33425-127">Výstup:</span><span class="sxs-lookup"><span data-stu-id="33425-127">Output:</span></span>

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED              SIZE
aci-tutorial-app             latest              5c745774dfa9        39 seconds ago       68.1 MB
```

## <a name="run-the-container-locally"></a><span data-ttu-id="33425-128">Místní spuštění kontejneru</span><span class="sxs-lookup"><span data-stu-id="33425-128">Run the container locally</span></span>

<span data-ttu-id="33425-129">Než se pokusíte kontejner nasadit do služby Azure Container Instances, spusťte ho místně, abyste ověřili, že funguje.</span><span class="sxs-lookup"><span data-stu-id="33425-129">Before you try deploying the container to Azure Container Instances, run it locally to confirm that it works.</span></span> <span data-ttu-id="33425-130">Přepínač `-d` umožňuje spuštění kontejneru na pozadí, zatímco přepínač `-p` umožňuje mapování libovolného portu vašeho počítače na port 80 kontejneru.</span><span class="sxs-lookup"><span data-stu-id="33425-130">The `-d` switch lets the container run in the background, while `-p` allows you to map an arbitrary port on your compute to port 80 in the container.</span></span>

```bash
docker run -d -p 8080:80 aci-tutorial-app
```

<span data-ttu-id="33425-131">Otevřete v prohlížeči adresu http://localhost:8080 a ověřte, že je kontejner spuštěný.</span><span class="sxs-lookup"><span data-stu-id="33425-131">Open the browser to http://localhost:8080 to confirm that the container is running.</span></span>

![Místní spuštění aplikace v prohlížeči][aci-tutorial-app-local]

## <a name="next-steps"></a><span data-ttu-id="33425-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="33425-133">Next steps</span></span>

<span data-ttu-id="33425-134">V tomto kurzu jste vytvořili image kontejneru, kterou je možné nasadit do služby Azure Container Instances.</span><span class="sxs-lookup"><span data-stu-id="33425-134">In this tutorial, you created a container image that can be deployed to Azure Container Instances.</span></span> <span data-ttu-id="33425-135">Dokončili jste následující kroky:</span><span class="sxs-lookup"><span data-stu-id="33425-135">The following steps were completed:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="33425-136">Klonování zdroje aplikace z GitHubu</span><span class="sxs-lookup"><span data-stu-id="33425-136">Cloning the application source from GitHub</span></span>  
> * <span data-ttu-id="33425-137">Vytváření imagí kontejnerů ze zdroje aplikace</span><span class="sxs-lookup"><span data-stu-id="33425-137">Creating container images from application source</span></span>
> * <span data-ttu-id="33425-138">Místní testování kontejneru</span><span class="sxs-lookup"><span data-stu-id="33425-138">Testing the container locally</span></span>

<span data-ttu-id="33425-139">Přejděte k dalšímu kurzu, ve kterém se seznámíte s ukládáním imagí kontejnerů ve službě Azure Container Registry.</span><span class="sxs-lookup"><span data-stu-id="33425-139">Advance to the next tutorial to learn about storing container images in an Azure Container Registry.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="33425-140">Nahrávání imagí do služby Azure Container Registry</span><span class="sxs-lookup"><span data-stu-id="33425-140">Push images to Azure Container Registry</span></span>](./container-instances-tutorial-prepare-acr.md)

<!-- LINKS -->
[dockerhub-nodeimage]: https://hub.docker.com/r/library/node/tags/8.2.0-alpine/

<!--- IMAGES --->
[aci-tutorial-app]:./media/container-instances-quickstart/aci-app-browser.png
[aci-tutorial-app-local]: ./media/container-instances-tutorial-prepare-app/aci-app-browser-local.png