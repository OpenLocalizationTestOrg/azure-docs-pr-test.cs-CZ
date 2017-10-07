---
title: "aaaAzure instancí kontejnerů kurz – Příprava aplikace | Dokumentace Azure"
description: "Příprava aplikace pro nasazení tooAzure instancí kontejnerů"
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
ms.openlocfilehash: 406ba796e5fefb1527f2e894cc3f7bbd8f7a5fd1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-container-for-deployment-tooazure-container-instances"></a><span data-ttu-id="1c444-103">Vytváření kontejneru pro nasazení tooAzure instancí kontejnerů</span><span class="sxs-lookup"><span data-stu-id="1c444-103">Create container for deployment tooAzure Container Instances</span></span>

<span data-ttu-id="1c444-104">Azure Container Instances umožňuje nasazení kontejnerů Dockeru na infrastrukturu Azure bez zřizování virtuálních počítačů nebo využívání služby vyšší úrovně.</span><span class="sxs-lookup"><span data-stu-id="1c444-104">Azure Container Instances enables deployment of Docker containers onto Azure infrastructure without provisioning any virtual machines or adopting any higher-level service.</span></span> <span data-ttu-id="1c444-105">V tomto kurzu vytvoříte jednoduchou webovou aplikaci v Node.js a zabalíte ji do kontejneru, který bude možné spustit pomocí služby Azure Container Instances.</span><span class="sxs-lookup"><span data-stu-id="1c444-105">In this tutorial, you will build a simple web application in Node.js and package it in a container that can be run using Azure Container Instances.</span></span> <span data-ttu-id="1c444-106">Společně probereme:</span><span class="sxs-lookup"><span data-stu-id="1c444-106">We will cover:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1c444-107">Klonování zdroje aplikace z GitHubu</span><span class="sxs-lookup"><span data-stu-id="1c444-107">Cloning application source from GitHub</span></span>  
> * <span data-ttu-id="1c444-108">Vytváření imagí kontejnerů ze zdroje aplikace</span><span class="sxs-lookup"><span data-stu-id="1c444-108">Creating container images from application source</span></span>
> * <span data-ttu-id="1c444-109">Testování obrázků hello v místním prostředí Docker</span><span class="sxs-lookup"><span data-stu-id="1c444-109">Testing hello images in a local Docker environment</span></span>

<span data-ttu-id="1c444-110">V následujících kurzech nahrát váš obrázek tooan registru kontejner Azure a pak je nasadit tooAzure kontejner instancí.</span><span class="sxs-lookup"><span data-stu-id="1c444-110">In subsequent tutorials, you will upload your image tooan Azure Container Registry, and then deploy them tooAzure Container Instances.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="1c444-111">Než začnete</span><span class="sxs-lookup"><span data-stu-id="1c444-111">Before you begin</span></span>

<span data-ttu-id="1c444-112">V tomto kurzu se předpokládá základní znalost klíčových konceptů Dockeru, jako jsou kontejnery, image kontejnerů a základní příkazy Dockeru.</span><span class="sxs-lookup"><span data-stu-id="1c444-112">This tutorial assumes a basic understanding of core Docker concepts such as containers, container images, and basic docker commands.</span></span> <span data-ttu-id="1c444-113">V případě potřeby najdete základní informace o kontejnerech v článku [Get started with Docker]( https://docs.docker.com/get-started/) (Začínáme s Dockerem).</span><span class="sxs-lookup"><span data-stu-id="1c444-113">If needed, see [Get started with Docker]( https://docs.docker.com/get-started/) for a primer on container basics.</span></span> 

<span data-ttu-id="1c444-114">toocomplete tohoto kurzu budete potřebovat Docker vývojovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="1c444-114">toocomplete this tutorial, you need a Docker development environment.</span></span> <span data-ttu-id="1c444-115">Docker nabízí balíčky pro snadnou konfiguraci Dockeru na jakémkoli systému [Mac](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) nebo [Linux](https://docs.docker.com/engine/installation/#supported-platforms).</span><span class="sxs-lookup"><span data-stu-id="1c444-115">Docker provides packages that easily configure Docker on any [Mac](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/), or [Linux](https://docs.docker.com/engine/installation/#supported-platforms) system.</span></span>

## <a name="get-application-code"></a><span data-ttu-id="1c444-116">Získání kódu aplikace</span><span class="sxs-lookup"><span data-stu-id="1c444-116">Get application code</span></span>

<span data-ttu-id="1c444-117">Ukázka Hello v tomto kurzu zahrnuje jednoduchou webovou aplikaci součástí [Node.js](http://nodejs.org).</span><span class="sxs-lookup"><span data-stu-id="1c444-117">hello sample in this tutorial includes a simple web application built in [Node.js](http://nodejs.org).</span></span> <span data-ttu-id="1c444-118">aplikace Hello slouží statické stránky HTML a vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="1c444-118">hello app serves a static HTML page and looks like this:</span></span>

![Ukázková aplikace zobrazená v prohlížeči][aci-tutorial-app]

<span data-ttu-id="1c444-120">Ukázka použití git toodownload hello:</span><span class="sxs-lookup"><span data-stu-id="1c444-120">Use git toodownload hello sample:</span></span>

```bash
git clone https://github.com/Azure-Samples/aci-helloworld.git
```

## <a name="build-hello-container-image"></a><span data-ttu-id="1c444-121">Sestavení hello kontejneru bitové kopie</span><span class="sxs-lookup"><span data-stu-id="1c444-121">Build hello container image</span></span>

<span data-ttu-id="1c444-122">Hello zadaná v úložišti hello ukázkový soubor Docker ukazuje, jak je integrovaná hello kontejneru.</span><span class="sxs-lookup"><span data-stu-id="1c444-122">hello Dockerfile provided in hello sample repo shows how hello container is built.</span></span> <span data-ttu-id="1c444-123">Spuštění z [oficiální Node.js image] [ dockerhub-nodeimage] na základě [Alpine Linux](https://alpinelinux.org/), malé rozdělení, na které se nehodí toouse s kontejnery.</span><span class="sxs-lookup"><span data-stu-id="1c444-123">It starts from an [official Node.js image][dockerhub-nodeimage] based on [Alpine Linux](https://alpinelinux.org/), a small distribution that is well suited toouse with containers.</span></span> <span data-ttu-id="1c444-124">Ji pak zkopíruje soubory aplikace hello do kontejneru hello, nainstaluje závislosti s využitím hello Node Package Manager a nakonec spustí aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="1c444-124">It then copies hello application files into hello container, installs dependencies using hello Node Package Manager, and finally starts hello application.</span></span>

```
FROM node:8.2.0-alpine
RUN mkdir -p /usr/src/app
COPY ./app/* /usr/src/app/
WORKDIR /usr/src/app
RUN npm install
CMD node /usr/src/app/index.js
```

<span data-ttu-id="1c444-125">Použití hello `docker build` toocreate hello kontejneru obrázek příkazu, označování jej jako *aci – kurz aplikace*:</span><span class="sxs-lookup"><span data-stu-id="1c444-125">Use hello `docker build` command toocreate hello container image, tagging it as *aci-tutorial-app*:</span></span>

```bash
docker build ./aci-helloworld -t aci-tutorial-app
```

<span data-ttu-id="1c444-126">Použití hello `docker images` toosee hello vytvořené bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="1c444-126">Use hello `docker images` toosee hello built image:</span></span>

```bash
docker images
```

<span data-ttu-id="1c444-127">Výstup:</span><span class="sxs-lookup"><span data-stu-id="1c444-127">Output:</span></span>

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED              SIZE
aci-tutorial-app             latest              5c745774dfa9        39 seconds ago       68.1 MB
```

## <a name="run-hello-container-locally"></a><span data-ttu-id="1c444-128">Spusťte kontejner hello místně</span><span class="sxs-lookup"><span data-stu-id="1c444-128">Run hello container locally</span></span>

<span data-ttu-id="1c444-129">Před nasazením hello kontejneru tooAzure kontejner instancí se jej spusťte místně tooconfirm, zda funguje.</span><span class="sxs-lookup"><span data-stu-id="1c444-129">Before you try deploying hello container tooAzure Container Instances, run it locally tooconfirm that it works.</span></span> <span data-ttu-id="1c444-130">Hello `-d` přepínač umožňuje hello kontejner spuštěny hello pozadí, zatímco `-p` vám umožní toomap na vaše výpočetní tooport 80 kontejneru hello libovolný port.</span><span class="sxs-lookup"><span data-stu-id="1c444-130">hello `-d` switch lets hello container run in hello background, while `-p` allows you toomap an arbitrary port on your compute tooport 80 in hello container.</span></span>

```bash
docker run -d -p 8080:80 aci-tutorial-app
```

<span data-ttu-id="1c444-131">Otevřete hello tooconfirm toohttp://localhost:8080 prohlížeče, který hello kontejneru je spuštěna.</span><span class="sxs-lookup"><span data-stu-id="1c444-131">Open hello browser toohttp://localhost:8080 tooconfirm that hello container is running.</span></span>

![Spuštěné aplikaci hello místně v prohlížeči hello][aci-tutorial-app-local]

## <a name="next-steps"></a><span data-ttu-id="1c444-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1c444-133">Next steps</span></span>

<span data-ttu-id="1c444-134">V tomto kurzu jste vytvořili kontejner bitovou kopii, která může být nasazený tooAzure kontejner instancí.</span><span class="sxs-lookup"><span data-stu-id="1c444-134">In this tutorial, you created a container image that can be deployed tooAzure Container Instances.</span></span> <span data-ttu-id="1c444-135">byly dokončeny Hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="1c444-135">hello following steps were completed:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1c444-136">Klonování zdroj aplikace hello z Githubu</span><span class="sxs-lookup"><span data-stu-id="1c444-136">Cloning hello application source from GitHub</span></span>  
> * <span data-ttu-id="1c444-137">Vytváření imagí kontejnerů ze zdroje aplikace</span><span class="sxs-lookup"><span data-stu-id="1c444-137">Creating container images from application source</span></span>
> * <span data-ttu-id="1c444-138">Testování hello kontejneru místně</span><span class="sxs-lookup"><span data-stu-id="1c444-138">Testing hello container locally</span></span>

<span data-ttu-id="1c444-139">Posunutí další kurz toolearn toohello o ukládání bitových kopií kontejneru v registru kontejneru služby Azure.</span><span class="sxs-lookup"><span data-stu-id="1c444-139">Advance toohello next tutorial toolearn about storing container images in an Azure Container Registry.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="1c444-140">Push bitové kopie tooAzure registru kontejneru</span><span class="sxs-lookup"><span data-stu-id="1c444-140">Push images tooAzure Container Registry</span></span>](./container-instances-tutorial-prepare-acr.md)

<!-- LINKS -->
[dockerhub-nodeimage]: https://hub.docker.com/r/library/node/tags/8.2.0-alpine/

<!--- IMAGES --->
[aci-tutorial-app]:./media/container-instances-quickstart/aci-app-browser.png
[aci-tutorial-app-local]: ./media/container-instances-tutorial-prepare-app/aci-app-browser-local.png