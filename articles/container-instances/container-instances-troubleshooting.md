---
title: "aaaTroubleshooting instancí kontejnerů Azure"
description: "Zjistěte, jak tootroubleshoot problémy s instancemi Azure kontejneru"
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
ms.openlocfilehash: dfec636a0a174c74a6f2e9d9c4da6e871f8d2fda
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-deployment-issues-with-azure-container-instances"></a><span data-ttu-id="30c35-103">Řešení potíží s nasazením s instancemi Azure kontejneru</span><span class="sxs-lookup"><span data-stu-id="30c35-103">Troubleshoot deployment issues with Azure Container Instances</span></span>

<span data-ttu-id="30c35-104">Tento článek ukazuje, jak tootroubleshoot problémy při nasazení tooAzure kontejnery instancí kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="30c35-104">This article shows how tootroubleshoot issues when deploying containers tooAzure Container Instances.</span></span> <span data-ttu-id="30c35-105">Popisuje také některé hello běžné problémy, které může spustíte do.</span><span class="sxs-lookup"><span data-stu-id="30c35-105">It also describes some of hello common issues you may run into.</span></span>

## <a name="getting-diagnostic-events"></a><span data-ttu-id="30c35-106">Získání diagnostických událostí</span><span class="sxs-lookup"><span data-stu-id="30c35-106">Getting diagnostic events</span></span>

<span data-ttu-id="30c35-107">tooview protokolů z vašeho kódu aplikace v rámci kontejneru, můžete použít hello [az kontejneru protokoly](/cli/azure/container#logs) příkaz.</span><span class="sxs-lookup"><span data-stu-id="30c35-107">tooview logs from your application code within a container, you can use hello [az container logs](/cli/azure/container#logs) command.</span></span> <span data-ttu-id="30c35-108">Ale pokud vaše kontejneru není úspěšně nasazena, je třeba tooreview hello diagnostické informace poskytované poskytovatelem prostředků Azure kontejner instancí hello.</span><span class="sxs-lookup"><span data-stu-id="30c35-108">But if your container does not deploy successfully, you need tooreview hello diagnostic information provided by hello Azure Container Instances resource provider.</span></span> <span data-ttu-id="30c35-109">tooview hello události pro váš kontejner, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="30c35-109">tooview hello events for your container, run hello following command:</span></span>

```azurecli-interactive
az container show -n mycontainername -g myresourcegroup
```

<span data-ttu-id="30c35-110">výstup Hello zahrnuje hello základní vlastnosti kontejneru, společně s událostí nasazení:</span><span class="sxs-lookup"><span data-stu-id="30c35-110">hello output includes hello core properties of your container, along with deployment events:</span></span>

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

## <a name="common-deployment-issues"></a><span data-ttu-id="30c35-111">Běžné problémy při nasazení</span><span class="sxs-lookup"><span data-stu-id="30c35-111">Common deployment issues</span></span>

<span data-ttu-id="30c35-112">Tento účet pro většinu chyb v nasazení existuje několik běžných problémů.</span><span class="sxs-lookup"><span data-stu-id="30c35-112">There are a few common issues that account for most errors in deployment.</span></span>

### <a name="unable-toopull-image"></a><span data-ttu-id="30c35-113">Obrázek nelze toopull</span><span class="sxs-lookup"><span data-stu-id="30c35-113">Unable toopull image</span></span>

<span data-ttu-id="30c35-114">Pokud Azure kontejner instancí je nelze toopull bitové kopie na začátku se pokusí po nějakou dobu před selháním nakonec.</span><span class="sxs-lookup"><span data-stu-id="30c35-114">If Azure Container Instances is unable toopull your image initially, it retries for some period before eventually failing.</span></span> <span data-ttu-id="30c35-115">Pokud nelze načíst obrázek hello, jsou uvedeny událostmi, jako je hello následující:</span><span class="sxs-lookup"><span data-stu-id="30c35-115">If hello image cannot be pulled, events like hello following are shown:</span></span>

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
    "message": "Failed: Failed toopull image \"microsoft/aci-hellowrld\": rpc error: code 2 desc Error: image microsoft/aci-hellowrld:latest not found",
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

<span data-ttu-id="30c35-116">tooresolve, odstraňte hello kontejneru a opakujte vaše nasazení, platící zvýšené pozornosti, že je správně zadán název bitové kopie hello.</span><span class="sxs-lookup"><span data-stu-id="30c35-116">tooresolve, delete hello container and retry your deployment, paying close attention that you have typed hello image name correctly.</span></span>

### <a name="container-continually-exits-and-restarts"></a><span data-ttu-id="30c35-117">Ukončení a restartování průběžně kontejneru</span><span class="sxs-lookup"><span data-stu-id="30c35-117">Container continually exits and restarts</span></span>

<span data-ttu-id="30c35-118">Kontejner instancí Azure v současné době podporuje pouze dlouhodobé služby.</span><span class="sxs-lookup"><span data-stu-id="30c35-118">Currently, Azure Container Instances only supports long-running services.</span></span> <span data-ttu-id="30c35-119">Pokud vaše kontejneru používá toocompletion a ukončí, automaticky restartuje a znovu spustí.</span><span class="sxs-lookup"><span data-stu-id="30c35-119">If your container runs toocompletion and exits, it automatically restarts and runs again.</span></span> <span data-ttu-id="30c35-120">V takovém případě se zobrazí událostmi, jako je následující.</span><span class="sxs-lookup"><span data-stu-id="30c35-120">If this happens, events like those following are shown.</span></span> <span data-ttu-id="30c35-121">Poznámka: Tento kontejner hello úspěšně spustí a potom rychle restartuje.</span><span class="sxs-lookup"><span data-stu-id="30c35-121">Note that hello container successfully starts, then quickly restarts.</span></span> <span data-ttu-id="30c35-122">zahrnuje technologie Hello kontejner instancí rozhraní API `retryCount` restartování vlastnost, která ukazuje, jak často konkrétním kontejneru.</span><span class="sxs-lookup"><span data-stu-id="30c35-122">hello Container Instances API includes a `retryCount` property that shows how many times a particular container has restarted.</span></span>

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
> <span data-ttu-id="30c35-123">Většina kontejneru bitových kopií pro Linuxových distribucích nastavit prostředí, jako je například bash, jako výchozí příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="30c35-123">Most container images for Linux distributions set a shell, such as bash, as hello default command.</span></span> <span data-ttu-id="30c35-124">Vzhledem k tomu, že prostředí svoje vlastní není služba dlouho běžící, tyto kontejnery okamžitě ukončit a spadají do smyčku restartování.</span><span class="sxs-lookup"><span data-stu-id="30c35-124">Since a shell on its own is not a long-running service, these containers immediately exit and fall into a restart loop.</span></span>

### <a name="container-takes-a-long-time-toostart"></a><span data-ttu-id="30c35-125">Kontejner trvá dlouho toostart</span><span class="sxs-lookup"><span data-stu-id="30c35-125">Container takes a long time toostart</span></span>

<span data-ttu-id="30c35-126">Pokud vaše kontejneru trvá dlouho toostart, ale nakonec úspěšná, spusťte prohlížením hello velikost bitové kopie kontejneru.</span><span class="sxs-lookup"><span data-stu-id="30c35-126">If your container takes a long time toostart, but eventually succeeds, start by looking at hello size of your container image.</span></span> <span data-ttu-id="30c35-127">Vzhledem k tomu instancí kontejnerů Azure vrátí kontejner image na vyžádání, je čas spuštění hello zaznamenáte přímo související tooits velikost.</span><span class="sxs-lookup"><span data-stu-id="30c35-127">Because Azure Container Instances pulls your container image on demand, hello startup time you experience is directly related tooits size.</span></span>

<span data-ttu-id="30c35-128">Můžete zobrazit hello velikost bitové kopie kontejneru pomocí příkazového řádku Dockeru hello:</span><span class="sxs-lookup"><span data-stu-id="30c35-128">You can view hello size of your container image using hello Docker CLI:</span></span>

```bash
docker images
```

<span data-ttu-id="30c35-129">Výstup:</span><span class="sxs-lookup"><span data-stu-id="30c35-129">Output:</span></span>

```bash
REPOSITORY                             TAG                 IMAGE ID            CREATED             SIZE
microsoft/aci-helloworld               latest              7f78509b568e        13 days ago         68.1MB
```

<span data-ttu-id="30c35-130">malá velikost obrázků klíče tookeeping Hello zajišťuje, že finální image neobsahuje nic, který není nutný za běhu.</span><span class="sxs-lookup"><span data-stu-id="30c35-130">hello key tookeeping image sizes small is ensuring that your final image does not contain anything that is not required at runtime.</span></span> <span data-ttu-id="30c35-131">Toto je s jedním ze způsobů toodo [více fáze sestavení](https://docs.docker.com/engine/userguide/eng-image/multistage-build/).</span><span class="sxs-lookup"><span data-stu-id="30c35-131">One way toodo this is with [multi-stage builds](https://docs.docker.com/engine/userguide/eng-image/multistage-build/).</span></span> <span data-ttu-id="30c35-132">Více fáze sestavení bylo snadné tooensure hello finální image obsahuje pouze hello artefakty potřebné pro aplikaci, a nikoli jakákoliv hello navíc obsahu, kterou nebyla nutná v čase vytvoření buildu.</span><span class="sxs-lookup"><span data-stu-id="30c35-132">Multi-stage builds make it easy tooensure that hello final image contains only hello artifacts you need for your application, and not any of hello extra content that was required at build time.</span></span>

<span data-ttu-id="30c35-133">Hello další způsob tooreduce hello dopad hello vyžádání bitové kopie na vaše kontejneru spuštění je toohost hello kontejneru image pomocí hello registru kontejner Azure v hello stejné oblasti, kde chcete toouse Azure kontejner instancí.</span><span class="sxs-lookup"><span data-stu-id="30c35-133">hello other way tooreduce hello impact of hello image pull on your container's startup time is toohost hello container image using hello Azure Container Registry in hello same region where you intend toouse Azure Container Instances.</span></span> <span data-ttu-id="30c35-134">Hello síťové cestě, která hello kontejneru image musí tootravel, výrazně zkrátit dobu stahování hello se zkrátí.</span><span class="sxs-lookup"><span data-stu-id="30c35-134">This shortens hello network path that hello container image needs tootravel, significantly shortening hello download time.</span></span>
