---
title: "kurz instancí kontejnerů aaaAzure – nasazení aplikace | Microsoft Docs"
description: "Kurz pro Azure instancí kontejnerů – nasazení aplikace"
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
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/04/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: b9fb098d9491e1073f0be4b14a0b9b1a18f16095
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-container-tooazure-container-instances"></a><span data-ttu-id="c2a5e-103">Nasazení tooAzure kontejner instancí kontejnerů</span><span class="sxs-lookup"><span data-stu-id="c2a5e-103">Deploy a container tooAzure Container Instances</span></span>

<span data-ttu-id="c2a5e-104">Toto je hello poslední kurzu třemi částmi.</span><span class="sxs-lookup"><span data-stu-id="c2a5e-104">This is hello last of a three-part tutorial.</span></span> <span data-ttu-id="c2a5e-105">V předchozích částech [bitovou kopii kontejner byl vytvořen](container-instances-tutorial-prepare-app.md) a [nabídnutých tooan registru kontejner Azure](container-instances-tutorial-prepare-acr.md).</span><span class="sxs-lookup"><span data-stu-id="c2a5e-105">In previous sections, [a container image was created](container-instances-tutorial-prepare-app.md) and [pushed tooan Azure Container Registry](container-instances-tutorial-prepare-acr.md).</span></span> <span data-ttu-id="c2a5e-106">Tato část dokončení kurzu hello nasazením hello kontejneru tooAzure instancí kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="c2a5e-106">This section completes hello tutorial by deploying hello container tooAzure Container Instances.</span></span> <span data-ttu-id="c2a5e-107">Dokončit krokům patří:</span><span class="sxs-lookup"><span data-stu-id="c2a5e-107">Steps completed include:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c2a5e-108">Definování skupinu kontejneru pomocí šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="c2a5e-108">Defining a container group using an Azure Resource Manager template</span></span>
> * <span data-ttu-id="c2a5e-109">Nasazení pomocí rozhraní příkazového řádku Azure hello skupina kontejneru hello</span><span class="sxs-lookup"><span data-stu-id="c2a5e-109">Deploying hello container group using hello Azure CLI</span></span>
> * <span data-ttu-id="c2a5e-110">Protokoly kontejner zobrazení</span><span class="sxs-lookup"><span data-stu-id="c2a5e-110">Viewing container logs</span></span>

## <a name="deploy-hello-container-using-hello-azure-cli"></a><span data-ttu-id="c2a5e-111">Nasazení pomocí rozhraní příkazového řádku Azure hello kontejneru hello</span><span class="sxs-lookup"><span data-stu-id="c2a5e-111">Deploy hello container using hello Azure CLI</span></span>

<span data-ttu-id="c2a5e-112">Hello rozhraní příkazového řádku Azure umožňuje nasazení kontejneru tooAzure kontejner instancí v jednom příkazu.</span><span class="sxs-lookup"><span data-stu-id="c2a5e-112">hello Azure CLI enables deployment of a container tooAzure Container Instances in a single command.</span></span> <span data-ttu-id="c2a5e-113">Vzhledem k tomu, že je hello kontejneru image umístěná v hello privátní registru kontejner Azure, je nutné zahrnout požadované tooaccess pověření hello ho.</span><span class="sxs-lookup"><span data-stu-id="c2a5e-113">Since hello container image is hosted in hello private Azure Container Registry, you must include hello credentials required tooaccess it.</span></span> <span data-ttu-id="c2a5e-114">V případě potřeby se můžete dotazovat je jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="c2a5e-114">If necessary, you can query them as shown below.</span></span>

<span data-ttu-id="c2a5e-115">Kontejner registru přihlášení server (aktualizace nahraďte názvem registru):</span><span class="sxs-lookup"><span data-stu-id="c2a5e-115">Container registry login server (update with your registry name):</span></span>

```azurecli-interactive
az acr show --name <acrName> --query loginServer
```

<span data-ttu-id="c2a5e-116">Kontejner registru heslo:</span><span class="sxs-lookup"><span data-stu-id="c2a5e-116">Container registry password:</span></span>

```azurecli-interactive
az acr credential show --name <acrName> --query "passwords[0].value"
```

<span data-ttu-id="c2a5e-117">toodeploy kontejneru image z registru hello kontejneru a zdroj žádosti o 1 jádro procesoru a 1GB paměti, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="c2a5e-117">toodeploy your container image from hello container registry with a resource request of 1 CPU core and 1GB of memory, run hello following command:</span></span>

```azurecli-interactive
az container create --name aci-tutorial-app --image <acrLoginServer>/aci-tutorial-app:v1 --cpu 1 --memory 1 --registry-password <acrPassword> --ip-address public -g myResourceGroup
```

<span data-ttu-id="c2a5e-118">Během pár sekund zobrazí se první odpověď ze Správce prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="c2a5e-118">Within a few seconds, you will receive an initial response from Azure Resource Manager.</span></span> <span data-ttu-id="c2a5e-119">Stav hello tooview hello nasazení, použijte:</span><span class="sxs-lookup"><span data-stu-id="c2a5e-119">tooview hello state of hello deployment, use:</span></span>

```azurecli-interactive
az container show --name aci-tutorial-app --resource-group myResourceGroup --query state
```

<span data-ttu-id="c2a5e-120">Abychom mohli pokračovat, dokud hello stav se změní z spuštěním tohoto příkazu *čekající* příliš*systémem*.</span><span class="sxs-lookup"><span data-stu-id="c2a5e-120">We can continue running this command until hello state changes from *pending* too*running*.</span></span> <span data-ttu-id="c2a5e-121">Potom jsme můžete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="c2a5e-121">Then we can proceed.</span></span>

## <a name="view-hello-application-and-container-logs"></a><span data-ttu-id="c2a5e-122">Zobrazit protokoly aplikací a kontejner hello</span><span class="sxs-lookup"><span data-stu-id="c2a5e-122">View hello application and container logs</span></span>

<span data-ttu-id="c2a5e-123">Po úspěšné nasazení hello, otevřete váš prohlížeč toohello IP adresu ukazuje výstup hello hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="c2a5e-123">Once hello deployment succeeds, open your browser toohello IP address shown in hello output of hello following command:</span></span>

```bash
az container show --name aci-tutorial-app --resource-group myResourceGroup --query ipAddress.ip
```

```json
"13.88.176.27"
```

![Hello world aplikaci v prohlížeči hello][aci-app-browser]

<span data-ttu-id="c2a5e-125">Můžete také zobrazit výstup hello protokolu hello kontejneru:</span><span class="sxs-lookup"><span data-stu-id="c2a5e-125">You can also view hello log output of hello container:</span></span>

```azurecli-interactive
az container logs --name aci-tutorial-app -g myResourceGroup
```

<span data-ttu-id="c2a5e-126">Výstup:</span><span class="sxs-lookup"><span data-stu-id="c2a5e-126">Output:</span></span>

```bash
listening on port 80
::ffff:10.240.0.4 - - [21/Jul/2017:06:00:02 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
::ffff:10.240.0.4 - - [21/Jul/2017:06:00:02 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://13.88.176.27/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
```

## <a name="next-steps"></a><span data-ttu-id="c2a5e-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c2a5e-127">Next steps</span></span>

<span data-ttu-id="c2a5e-128">V tomto kurzu jste dokončili proces hello nasazení vaší tooAzure kontejnery instancí kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="c2a5e-128">In this tutorial, you completed hello process of deploying your containers tooAzure Container Instances.</span></span> <span data-ttu-id="c2a5e-129">byly dokončeny Hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c2a5e-129">hello following steps were completed:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c2a5e-130">Nasazení kontejneru hello pomocí registru kontejner Azure hello hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="c2a5e-130">Deploying hello container from hello Azure Container Registry using hello Azure CLI</span></span>
> * <span data-ttu-id="c2a5e-131">Zobrazení aplikace hello v prohlížeči hello</span><span class="sxs-lookup"><span data-stu-id="c2a5e-131">Viewing hello application in hello browser</span></span>
> * <span data-ttu-id="c2a5e-132">Zobrazení hello kontejneru protokoly</span><span class="sxs-lookup"><span data-stu-id="c2a5e-132">Viewing hello container logs</span></span>

<!-- LINKS -->
[prepare-app]: ./container-instances-tutorial-prepare-app.md

<!-- IMAGES -->
[aci-app-browser]: ./media/container-instances-quickstart/aci-app-browser.png
