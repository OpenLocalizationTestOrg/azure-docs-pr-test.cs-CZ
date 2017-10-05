---
title: "Řešení potíží s instancí Azure kontejnerů"
description: "Zjistěte, jak vyřešit problémy s instancí kontejnerů Azure"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/03/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 86fa4b7dca7c362f95c0243a33f03d1f2dd3ab42
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="troubleshoot-deployment-issues-with-azure-container-instances"></a><span data-ttu-id="0b252-103">Řešení potíží s nasazením s instancemi Azure kontejneru</span><span class="sxs-lookup"><span data-stu-id="0b252-103">Troubleshoot deployment issues with Azure Container Instances</span></span>

<span data-ttu-id="0b252-104">Tento článek ukazuje, jak vyřešit problémy při nasazení kontejnerů do Azure kontejner instancí.</span><span class="sxs-lookup"><span data-stu-id="0b252-104">This article shows how to troubleshoot issues when deploying containers to Azure Container Instances.</span></span> <span data-ttu-id="0b252-105">Také popisuje některé běžné problémy, které může spustit do.</span><span class="sxs-lookup"><span data-stu-id="0b252-105">It also describes some of the common issues you may run into.</span></span>

## <a name="getting-diagnostic-events"></a><span data-ttu-id="0b252-106">Získání diagnostických událostí</span><span class="sxs-lookup"><span data-stu-id="0b252-106">Getting diagnostic events</span></span>

<span data-ttu-id="0b252-107">Chcete-li zobrazit protokoly z kódu aplikace v rámci kontejneru, můžete použít [az kontejneru protokoly](/cli/azure/container#logs) příkaz.</span><span class="sxs-lookup"><span data-stu-id="0b252-107">To view logs from your application code within a container, you can use the [az container logs](/cli/azure/container#logs) command.</span></span> <span data-ttu-id="0b252-108">Ale pokud vaše kontejneru není úspěšně nasazena, je potřeba zkontrolovat diagnostické informace poskytované poskytovatelem prostředků Azure kontejner instancí.</span><span class="sxs-lookup"><span data-stu-id="0b252-108">But if your container does not deploy successfully, you need to review the diagnostic information provided by the Azure Container Instances resource provider.</span></span> <span data-ttu-id="0b252-109">Pokud chcete zobrazit události pro vaše kontejneru, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="0b252-109">To view the events for your container, run the following command:</span></span>

```azurecli-interactive
az container show -n mycontainername -g myresourcegroup
```

<span data-ttu-id="0b252-110">Výstup obsahuje základní vlastnosti kontejneru, společně s událostí nasazení:</span><span class="sxs-lookup"><span data-stu-id="0b252-110">The output includes the core properties of your container, along with deployment events:</span></span>

```bash
{
  "containers": [
    {
      "command": null,
      "environmentVariables": [],
      "image": "microsoft/aci-helloworld",
      ...

      "events": [
      {
        "count": 1,
        "firstTimestamp": "2017-08-03T22:12:52+00:00",
        "lastTimestamp": "2017-08-03T22:12:52+00:00",
        "message": "Pulling: pulling image \"microsoft/aci-helloworld\"",
        "type": "Normal"
      },
      {
        "count": 1,
        "firstTimestamp": "2017-08-03T22:12:55+00:00",
        "lastTimestamp": "2017-08-03T22:12:55+00:00",
        "message": "Pulled: Successfully pulled image \"microsoft/aci-helloworld\"",
        "type": "Normal"
      },
      {
        "count": 1,
        "firstTimestamp": "2017-08-03T22:12:55+00:00",
        "lastTimestamp": "2017-08-03T22:12:55+00:00",
        "message": "Created: Created container with id 61602059d6c31529c27609ef4ec0c858b0a96150177fa045cf944d7cf8fbab69",
        "type": "Normal"
      },
      {
        "count": 1,
        "firstTimestamp": "2017-08-03T22:12:55+00:00",
        "lastTimestamp": "2017-08-03T22:12:55+00:00",
        "message": "Started: Started container with id 61602059d6c31529c27609ef4ec0c858b0a96150177fa045cf944d7cf8fbab69",
        "type": "Normal"
      }
    ],
    "name": "helloworld",
      "ports": [
        {
          "port": 80
        }
      ],
    ...
  ]
}
```

## <a name="common-deployment-issues"></a><span data-ttu-id="0b252-111">Běžné problémy při nasazení</span><span class="sxs-lookup"><span data-stu-id="0b252-111">Common deployment issues</span></span>

<span data-ttu-id="0b252-112">Tento účet pro většinu chyb v nasazení existuje několik běžných problémů.</span><span class="sxs-lookup"><span data-stu-id="0b252-112">There are a few common issues that account for most errors in deployment.</span></span>

### <a name="unable-to-pull-image"></a><span data-ttu-id="0b252-113">Nelze pro vyžádání obsahu image</span><span class="sxs-lookup"><span data-stu-id="0b252-113">Unable to pull image</span></span>

<span data-ttu-id="0b252-114">Pokud instance kontejner Azure nemůže původně vyžádání bitové kopie, se pokusí po nějakou dobu před selháním nakonec.</span><span class="sxs-lookup"><span data-stu-id="0b252-114">If Azure Container Instances is unable to pull your image initially, it retries for some period before eventually failing.</span></span> <span data-ttu-id="0b252-115">Pokud nelze načíst obrázek, jsou uvedeny událostmi, jako je následující:</span><span class="sxs-lookup"><span data-stu-id="0b252-115">If the image cannot be pulled, events like the following are shown:</span></span>

```bash
"events": [
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:19:31+00:00",
    "lastTimestamp": "2017-08-03T22:19:31+00:00",
    "message": "Pulling: pulling image \"microsoft/aci-hellowrld\"",
    "type": "Normal"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:19:32+00:00",
    "lastTimestamp": "2017-08-03T22:19:32+00:00",
    "message": "Failed: Failed to pull image \"microsoft/aci-hellowrld\": rpc error: code 2 desc Error: image microsoft/aci-hellowrld:latest not found",
    "type": "Warning"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:19:33+00:00",
    "lastTimestamp": "2017-08-03T22:19:33+00:00",
    "message": "BackOff: Back-off pulling image \"microsoft/aci-hellowrld\"",
    "type": "Normal"
  }
]
```

<span data-ttu-id="0b252-116">Vyřešit, odstraňte kontejneru a opakujte vaše nasazení, platící zvýšené pozornosti, že jste správně zadali název bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="0b252-116">To resolve, delete the container and retry your deployment, paying close attention that you have typed the image name correctly.</span></span>

### <a name="container-continually-exits-and-restarts"></a><span data-ttu-id="0b252-117">Ukončení a restartování průběžně kontejneru</span><span class="sxs-lookup"><span data-stu-id="0b252-117">Container continually exits and restarts</span></span>

<span data-ttu-id="0b252-118">Kontejner instancí Azure v současné době podporuje pouze dlouhodobé služby.</span><span class="sxs-lookup"><span data-stu-id="0b252-118">Currently, Azure Container Instances only supports long-running services.</span></span> <span data-ttu-id="0b252-119">Pokud vaše kontejneru se používá k dokončení a ukončí, automaticky restartuje a znovu spustí.</span><span class="sxs-lookup"><span data-stu-id="0b252-119">If your container runs to completion and exits, it automatically restarts and runs again.</span></span> <span data-ttu-id="0b252-120">V takovém případě se zobrazí událostmi, jako je následující.</span><span class="sxs-lookup"><span data-stu-id="0b252-120">If this happens, events like those following are shown.</span></span> <span data-ttu-id="0b252-121">Všimněte si, že kontejner úspěšně spustí a potom rychle restartuje.</span><span class="sxs-lookup"><span data-stu-id="0b252-121">Note that the container successfully starts, then quickly restarts.</span></span> <span data-ttu-id="0b252-122">Zahrnuje rozhraní API instancí kontejneru `retryCount` restartování vlastnost, která ukazuje, jak často konkrétním kontejneru.</span><span class="sxs-lookup"><span data-stu-id="0b252-122">The Container Instances API includes a `retryCount` property that shows how many times a particular container has restarted.</span></span>

```bash
"events": [
  {
    "count": 5,
    "firstTimestamp": "2017-08-03T22:21:55+00:00",
    "lastTimestamp": "2017-08-03T22:23:22+00:00",
    "message": "Pulling: pulling image \"alpine\"",
    "type": "Normal"
  },
  {
    "count": 5,
    "firstTimestamp": "2017-08-03T22:21:57+00:00",
    "lastTimestamp": "2017-08-03T22:23:23+00:00",
    "message": "Pulled: Successfully pulled image \"alpine\"",
    "type": "Normal"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:21:57+00:00",
    "lastTimestamp": "2017-08-03T22:21:57+00:00",
    "message": "Created: Created container with id ad2bf9bc51761c5f935260b4bab53b164d52d9cbc045b16afcb26fb4d14d0a70",
    "type": "Normal"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:21:57+00:00",
    "lastTimestamp": "2017-08-03T22:21:57+00:00",
    "message": "Started: Started container with id ad2bf9bc51761c5f935260b4bab53b164d52d9cbc045b16afcb26fb4d14d0a70",
    "type": "Normal"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:21:58+00:00",
    "lastTimestamp": "2017-08-03T22:21:58+00:00",
    "message": "Created: Created container with id 7687b9bd15dc01731fa66fc45f6f0241495600602dd03841e559453245e7f70b",
    "type": "Normal"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:21:58+00:00",
    "lastTimestamp": "2017-08-03T22:21:58+00:00",
    "message": "Started: Started container with id 7687b9bd15dc01731fa66fc45f6f0241495600602dd03841e559453245e7f70b",
    "type": "Normal"
  },
  {
    "count": 13,
    "firstTimestamp": "2017-08-03T22:21:59+00:00",
    "lastTimestamp": "2017-08-03T22:24:36+00:00",
    "message": "BackOff: Back-off restarting failed container",
    "type": "Warning"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:22:13+00:00",
    "lastTimestamp": "2017-08-03T22:22:13+00:00",
    "message": "Created: Created container with id 72e347e891290e238135e4a6b3078748ca25a1275dbbff30d8d214f026d89220",
    "type": "Normal"
  },
  ...
```

> [!NOTE]
> <span data-ttu-id="0b252-123">Většina kontejneru bitových kopií pro Linuxových distribucích prostředí, jako je například bash, nastavit jako výchozí příkaz.</span><span class="sxs-lookup"><span data-stu-id="0b252-123">Most container images for Linux distributions set a shell, such as bash, as the default command.</span></span> <span data-ttu-id="0b252-124">Vzhledem k tomu, že prostředí svoje vlastní není služba dlouho běžící, tyto kontejnery okamžitě ukončit a spadají do smyčku restartování.</span><span class="sxs-lookup"><span data-stu-id="0b252-124">Since a shell on its own is not a long-running service, these containers immediately exit and fall into a restart loop.</span></span>

### <a name="container-takes-a-long-time-to-start"></a><span data-ttu-id="0b252-125">Kontejner trvá dlouhou dobu spuštění</span><span class="sxs-lookup"><span data-stu-id="0b252-125">Container takes a long time to start</span></span>

<span data-ttu-id="0b252-126">Pokud vaše kontejneru trvá dlouhou dobu spuštění, ale nakonec úspěšná, začít hledáním na velikost bitové kopie kontejneru.</span><span class="sxs-lookup"><span data-stu-id="0b252-126">If your container takes a long time to start, but eventually succeeds, start by looking at the size of your container image.</span></span> <span data-ttu-id="0b252-127">Protože Azure kontejner instancí vrátí kontejner image na vyžádání, čas spuštění, které zaznamenáte přímo souvisí s jeho velikost.</span><span class="sxs-lookup"><span data-stu-id="0b252-127">Because Azure Container Instances pulls your container image on demand, the startup time you experience is directly related to its size.</span></span>

<span data-ttu-id="0b252-128">Velikost vaší image kontejneru pomocí rozhraní příkazového řádku Dockeru můžete zobrazit:</span><span class="sxs-lookup"><span data-stu-id="0b252-128">You can view the size of your container image using the Docker CLI:</span></span>

```bash
docker images
```

<span data-ttu-id="0b252-129">Výstup:</span><span class="sxs-lookup"><span data-stu-id="0b252-129">Output:</span></span>

```bash
REPOSITORY                             TAG                 IMAGE ID            CREATED             SIZE
microsoft/aci-helloworld               latest              7f78509b568e        13 days ago         68.1MB
```

<span data-ttu-id="0b252-130">Klíč k udržování velikosti obrázků malé zajišťuje, že finální image neobsahuje nic, který není nutný za běhu.</span><span class="sxs-lookup"><span data-stu-id="0b252-130">The key to keeping image sizes small is ensuring that your final image does not contain anything that is not required at runtime.</span></span> <span data-ttu-id="0b252-131">Jeden ze způsobů, jak provést toto je s [více fáze sestavení](https://docs.docker.com/engine/userguide/eng-image/multistage-build/).</span><span class="sxs-lookup"><span data-stu-id="0b252-131">One way to do this is with [multi-stage builds](https://docs.docker.com/engine/userguide/eng-image/multistage-build/).</span></span> <span data-ttu-id="0b252-132">Více fáze sestavení zkontrolujte usnadňují zajistěte, aby finální image obsahuje pouze artefakty potřebné pro vaši aplikaci a ne všechny nadbytečné obsahu, kterou nebyla nutná v čase vytvoření buildu.</span><span class="sxs-lookup"><span data-stu-id="0b252-132">Multi-stage builds make it easy to ensure that the final image contains only the artifacts you need for your application, and not any of the extra content that was required at build time.</span></span>

<span data-ttu-id="0b252-133">Jiný způsob, jak snížit dopad vyžádání obsahu bitové kopie na vaše kontejneru spuštění je hostitelem kontejneru image pomocí klíče registru kontejner Azure ve stejné oblasti, kde chcete používat Azure kontejner instancí.</span><span class="sxs-lookup"><span data-stu-id="0b252-133">The other way to reduce the impact of the image pull on your container's startup time is to host the container image using the Azure Container Registry in the same region where you intend to use Azure Container Instances.</span></span> <span data-ttu-id="0b252-134">To zkracuje síťové cestě, která bitovou kopii kontejneru je potřeba cestují, výrazně zkrátit dobu stahování.</span><span class="sxs-lookup"><span data-stu-id="0b252-134">This shortens the network path that the container image needs to travel, significantly shortening the download time.</span></span>