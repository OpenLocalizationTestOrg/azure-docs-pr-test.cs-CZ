---
title: "Kurz pro Azure instancí kontejnerů – nasazení aplikace | Microsoft Docs"
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
ms.openlocfilehash: 54151a5c1850ab7120fe666a46dc5dc99c0f5157
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="deploy-a-container-to-azure-container-instances"></a><span data-ttu-id="bd5fb-103">Nasazení do Azure kontejner instancí kontejneru</span><span class="sxs-lookup"><span data-stu-id="bd5fb-103">Deploy a container to Azure Container Instances</span></span>

<span data-ttu-id="bd5fb-104">Toto je poslední kurzu třemi částmi.</span><span class="sxs-lookup"><span data-stu-id="bd5fb-104">This is the last of a three-part tutorial.</span></span> <span data-ttu-id="bd5fb-105">V předchozích částech [bitovou kopii kontejner byl vytvořen](container-instances-tutorial-prepare-app.md) a [nabídnutých do služby Azure kontejneru registru](container-instances-tutorial-prepare-acr.md).</span><span class="sxs-lookup"><span data-stu-id="bd5fb-105">In previous sections, [a container image was created](container-instances-tutorial-prepare-app.md) and [pushed to an Azure Container Registry](container-instances-tutorial-prepare-acr.md).</span></span> <span data-ttu-id="bd5fb-106">Tato část dokončení tohoto kurzu nasazením kontejneru na kontejner instancí Azure.</span><span class="sxs-lookup"><span data-stu-id="bd5fb-106">This section completes the tutorial by deploying the container to Azure Container Instances.</span></span> <span data-ttu-id="bd5fb-107">Dokončit krokům patří:</span><span class="sxs-lookup"><span data-stu-id="bd5fb-107">Steps completed include:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="bd5fb-108">Definování skupinu kontejneru pomocí šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="bd5fb-108">Defining a container group using an Azure Resource Manager template</span></span>
> * <span data-ttu-id="bd5fb-109">Nasazení kontejneru skupiny, pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="bd5fb-109">Deploying the container group using the Azure CLI</span></span>
> * <span data-ttu-id="bd5fb-110">Protokoly kontejner zobrazení</span><span class="sxs-lookup"><span data-stu-id="bd5fb-110">Viewing container logs</span></span>

## <a name="deploy-the-container-using-the-azure-cli"></a><span data-ttu-id="bd5fb-111">Nasaďte kontejner pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="bd5fb-111">Deploy the container using the Azure CLI</span></span>

<span data-ttu-id="bd5fb-112">Rozhraní příkazového řádku Azure umožňuje nasazení kontejner Azure kontejner instancí v jednom příkazu.</span><span class="sxs-lookup"><span data-stu-id="bd5fb-112">The Azure CLI enables deployment of a container to Azure Container Instances in a single command.</span></span> <span data-ttu-id="bd5fb-113">Vzhledem k tomu, že je kontejner image umístěná v privátní registru kontejner Azure, musí zahrnovat přihlašovací údaje potřebné pro přístup k ní.</span><span class="sxs-lookup"><span data-stu-id="bd5fb-113">Since the container image is hosted in the private Azure Container Registry, you must include the credentials required to access it.</span></span> <span data-ttu-id="bd5fb-114">V případě potřeby se můžete dotazovat je jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="bd5fb-114">If necessary, you can query them as shown below.</span></span>

<span data-ttu-id="bd5fb-115">Kontejner registru přihlášení server (aktualizace nahraďte názvem registru):</span><span class="sxs-lookup"><span data-stu-id="bd5fb-115">Container registry login server (update with your registry name):</span></span>

```azurecli-interactive
az acr show --name <acrName> --query loginServer
```

<span data-ttu-id="bd5fb-116">Kontejner registru heslo:</span><span class="sxs-lookup"><span data-stu-id="bd5fb-116">Container registry password:</span></span>

```azurecli-interactive
az acr credential show --name <acrName> --query "passwords[0].value"
```

<span data-ttu-id="bd5fb-117">K nasazení bitové kopie kontejneru z registru kontejneru se žádost o prostředku procesorového 1 a 1GB paměti, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="bd5fb-117">To deploy your container image from the container registry with a resource request of 1 CPU core and 1GB of memory, run the following command:</span></span>

```azurecli-interactive
az container create --name aci-tutorial-app --image <acrLoginServer>/aci-tutorial-app:v1 --cpu 1 --memory 1 --registry-password <acrPassword> --ip-address public -g myResourceGroup
```

<span data-ttu-id="bd5fb-118">Během pár sekund zobrazí se první odpověď ze Správce prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="bd5fb-118">Within a few seconds, you will receive an initial response from Azure Resource Manager.</span></span> <span data-ttu-id="bd5fb-119">Pokud chcete zobrazit stav nasazení, použijte:</span><span class="sxs-lookup"><span data-stu-id="bd5fb-119">To view the state of the deployment, use:</span></span>

```azurecli-interactive
az container show --name aci-tutorial-app --resource-group myResourceGroup --query state
```

<span data-ttu-id="bd5fb-120">Abychom mohli pokračovat, dokud se stav změní z spuštěním tohoto příkazu *čekající* k *systémem*.</span><span class="sxs-lookup"><span data-stu-id="bd5fb-120">We can continue running this command until the state changes from *pending* to *running*.</span></span> <span data-ttu-id="bd5fb-121">Potom jsme můžete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="bd5fb-121">Then we can proceed.</span></span>

## <a name="view-the-application-and-container-logs"></a><span data-ttu-id="bd5fb-122">Zobrazit protokoly aplikací a kontejneru</span><span class="sxs-lookup"><span data-stu-id="bd5fb-122">View the application and container logs</span></span>

<span data-ttu-id="bd5fb-123">Po úspěšné nasazení, otevřete prohlížeč na IP adresu zobrazené ve výstupu příkazu:</span><span class="sxs-lookup"><span data-stu-id="bd5fb-123">Once the deployment succeeds, open your browser to the IP address shown in the output of the following command:</span></span>

```bash
az container show --name aci-tutorial-app --resource-group myResourceGroup --query ipAddress.ip
```

```json
"13.88.176.27"
```

![Hello world aplikace v prohlížeči][aci-app-browser]

<span data-ttu-id="bd5fb-125">Můžete také zobrazit výstup protokolu kontejneru:</span><span class="sxs-lookup"><span data-stu-id="bd5fb-125">You can also view the log output of the container:</span></span>

```azurecli-interactive
az container logs --name aci-tutorial-app -g myResourceGroup
```

<span data-ttu-id="bd5fb-126">Výstup:</span><span class="sxs-lookup"><span data-stu-id="bd5fb-126">Output:</span></span>

```bash
listening on port 80
::ffff:10.240.0.4 - - [21/Jul/2017:06:00:02 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
::ffff:10.240.0.4 - - [21/Jul/2017:06:00:02 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://13.88.176.27/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
```

## <a name="next-steps"></a><span data-ttu-id="bd5fb-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bd5fb-127">Next steps</span></span>

<span data-ttu-id="bd5fb-128">V tomto kurzu jste dokončili proces nasazení kontejnerů do Azure kontejner instancí.</span><span class="sxs-lookup"><span data-stu-id="bd5fb-128">In this tutorial, you completed the process of deploying your containers to Azure Container Instances.</span></span> <span data-ttu-id="bd5fb-129">Dokončili jste následující kroky:</span><span class="sxs-lookup"><span data-stu-id="bd5fb-129">The following steps were completed:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="bd5fb-130">Nasazení kontejneru z registru kontejner Azure pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="bd5fb-130">Deploying the container from the Azure Container Registry using the Azure CLI</span></span>
> * <span data-ttu-id="bd5fb-131">Zobrazení aplikace v prohlížeči</span><span class="sxs-lookup"><span data-stu-id="bd5fb-131">Viewing the application in the browser</span></span>
> * <span data-ttu-id="bd5fb-132">Prohlížení protokolů kontejneru</span><span class="sxs-lookup"><span data-stu-id="bd5fb-132">Viewing the container logs</span></span>

<!-- LINKS -->
[prepare-app]: ./container-instances-tutorial-prepare-app.md

<!-- IMAGES -->
[aci-app-browser]: ./media/container-instances-quickstart/aci-app-browser.png
