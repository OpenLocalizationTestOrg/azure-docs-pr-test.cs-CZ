---
title: "Vytvoření prvního kontejneru služby Azure Container Instances | Dokumentace Azure"
description: "Nasazení služby Azure Container Instances a zahájení práce"
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
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 905f69e5e1e237a31d9bb1e326969ec83292c244
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-your-first-container-in-azure-container-instances"></a><span data-ttu-id="34abf-103">Vytvoření prvního kontejneru ve službě Azure Container Instances</span><span class="sxs-lookup"><span data-stu-id="34abf-103">Create your first container in Azure Container Instances</span></span>

<span data-ttu-id="34abf-104">Azure Container Instances zjednodušuje vytváření a správu kontejnerů v Azure.</span><span class="sxs-lookup"><span data-stu-id="34abf-104">Azure Container Instances makes it easy to create and manage containers in Azure.</span></span> <span data-ttu-id="34abf-105">V tomto rychlém startu vytvoříte kontejner v Azure a zveřejníte ho na internetu s použitím veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="34abf-105">In this quick start, you will create a container in Azure and expose it to the internet with a public IP address.</span></span> <span data-ttu-id="34abf-106">K dokončení této operace stačí jediný příkaz.</span><span class="sxs-lookup"><span data-stu-id="34abf-106">This operation is completed in a single command.</span></span> <span data-ttu-id="34abf-107">Během několika sekund uvidíte ve svém prohlížeči toto:</span><span class="sxs-lookup"><span data-stu-id="34abf-107">Within just a few seconds, you will see this in your browser:</span></span>

![Aplikace nasazená pomocí služby Azure Container Instances zobrazená v prohlížeči][aci-app-browser]

<span data-ttu-id="34abf-109">Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.</span><span class="sxs-lookup"><span data-stu-id="34abf-109">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="34abf-110">Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku místně, musíte mít Azure CLI verze 2.0.12 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="34abf-110">If you choose to install and use the CLI locally, this quickstart requires that you are running the Azure CLI version 2.0.12 or later.</span></span> <span data-ttu-id="34abf-111">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="34abf-111">Run `az --version` to find the version.</span></span> <span data-ttu-id="34abf-112">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="34abf-112">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="create-a-resource-group"></a><span data-ttu-id="34abf-113">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="34abf-113">Create a resource group</span></span>

<span data-ttu-id="34abf-114">Služba Azure Container Instances je prostředek Azure a musí být umístěná ve skupině prostředků Azure, což je logická kolekce, ve které se nasazují a spravují prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="34abf-114">Azure Container Instances are Azure resources and must be placed in an Azure resource group, a logical collection into which Azure resources are deployed and managed.</span></span>

<span data-ttu-id="34abf-115">Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="34abf-115">Create a resource group with the [az group create](/cli/azure/group#create) command.</span></span> 

<span data-ttu-id="34abf-116">Následující příklad vytvoří skupinu prostředků *myResourceGroup* v umístění *eastus*.</span><span class="sxs-lookup"><span data-stu-id="34abf-116">The following example creates a resource group named *myResourceGroup* in the *eastus* location.</span></span>

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container"></a><span data-ttu-id="34abf-117">Vytvoření kontejneru</span><span class="sxs-lookup"><span data-stu-id="34abf-117">Create a container</span></span>

<span data-ttu-id="34abf-118">Kontejner můžete vytvořit zadáním názvu, image Dockeru a skupiny prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="34abf-118">You can create a container by providing a name, a Docker image, and an Azure resource group.</span></span> <span data-ttu-id="34abf-119">Volitelně můžete kontejner zveřejnit na internetu s použitím veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="34abf-119">You can optionally expose the container to the internet with a public IP address.</span></span> <span data-ttu-id="34abf-120">V tomto případě použijeme kontejner, který je hostitelem velmi jednoduché webové aplikace napsané v [Node.js](http://nodejs.org).</span><span class="sxs-lookup"><span data-stu-id="34abf-120">In this case, we'll use a container that hosts a very simple web app written in [Node.js](http://nodejs.org).</span></span>

```azurecli-interactive
az container create --name mycontainer --image microsoft/aci-helloworld --resource-group myResourceGroup --ip-address public 
```

<span data-ttu-id="34abf-121">Během několika sekund byste měli obdržet odpověď na váš požadavek.</span><span class="sxs-lookup"><span data-stu-id="34abf-121">Within a few seconds, you should get a response to your request.</span></span> <span data-ttu-id="34abf-122">Zpočátku bude kontejner ve stavu **Vytváření**, ale během několika sekund by se měl spustit.</span><span class="sxs-lookup"><span data-stu-id="34abf-122">Initially, the container will be in a **Creating** state, but it should start within a few seconds.</span></span> <span data-ttu-id="34abf-123">Stav můžete zkontrolovat pomocí příkazu `show`:</span><span class="sxs-lookup"><span data-stu-id="34abf-123">You can check the status using the `show` command:</span></span>

```azurecli-interactive
az container show --name mycontainer --resource-group myResourceGroup
```

<span data-ttu-id="34abf-124">V dolní části výstupu se zobrazí stav zřizování kontejneru a jeho IP adresa:</span><span class="sxs-lookup"><span data-stu-id="34abf-124">At the bottom of the output, you will see the container's provisioning state and its IP address:</span></span>

```json
...
"ipAddress": {
      "ip": "13.88.8.148",
      "ports": [
        {
          "port": 80,
          "protocol": "TCP"
        }
      ]
    },
    "osType": "Linux",
    "provisioningState": "Succeeded"
...
```

<span data-ttu-id="34abf-125">Jakmile se kontejner přesune do stavu **Úspěšné**, můžete k němu přistoupit v prohlížeči pomocí zadané IP adresy.</span><span class="sxs-lookup"><span data-stu-id="34abf-125">Once the container moves to the **Succeeded** state, you can reach it in the browser using the IP address provided.</span></span> 

![Aplikace nasazená pomocí služby Azure Container Instances zobrazená v prohlížeči][aci-app-browser]

## <a name="pull-the-container-logs"></a><span data-ttu-id="34abf-127">Vyžádání protokolů kontejneru</span><span class="sxs-lookup"><span data-stu-id="34abf-127">Pull the container logs</span></span>

<span data-ttu-id="34abf-128">Protokoly pro vytvořený kontejner si můžete vyžádat pomocí příkazu `logs`:</span><span class="sxs-lookup"><span data-stu-id="34abf-128">You can pull the logs for the container you created using the `logs` command:</span></span>

```azurecli-interactive
az container logs --name mycontainer --resource-group myResourceGroup
```

<span data-ttu-id="34abf-129">Výstup:</span><span class="sxs-lookup"><span data-stu-id="34abf-129">Output:</span></span>

```bash
listening on port 80
::ffff:10.240.255.105 - - [21/Jul/2017:00:01:46 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
::ffff:10.240.255.105 - - [21/Jul/2017:00:01:46 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://104.210.39.122/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
```

## <a name="delete-the-container"></a><span data-ttu-id="34abf-130">Odstranění kontejneru</span><span class="sxs-lookup"><span data-stu-id="34abf-130">Delete the container</span></span>

<span data-ttu-id="34abf-131">Až s kontejnerem skončíte, můžete ho odebrat pomocí příkazu `delete`:</span><span class="sxs-lookup"><span data-stu-id="34abf-131">When you are done with the container, you can remove it using the `delete` command:</span></span>

```azurecli-interactive
az container delete --name mycontainer --resource-group myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="34abf-132">Další kroky</span><span class="sxs-lookup"><span data-stu-id="34abf-132">Next steps</span></span>

<span data-ttu-id="34abf-133">Veškerý kód pro kontejner použitý v tomto rychlém startu je k dispozici [na GitHubu][app-github-repo] společně s příslušným souborem Dockerfile.</span><span class="sxs-lookup"><span data-stu-id="34abf-133">All of the code for the container used in this quick start is available [on GitHub][app-github-repo], along with its Dockerfile.</span></span> <span data-ttu-id="34abf-134">Pokud byste si chtěli sami vyzkoušet jeho sestavení a nasazení do služby Azure Container Instances pomocí služby Azure Container Registry, pokračujte na kurz služby Azure Container Instances.</span><span class="sxs-lookup"><span data-stu-id="34abf-134">If you'd like to try building it yourself and deploying it to Azure Container Instances using the Azure Container Registry, continue to the Azure Container Instances tutorial.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="34abf-135">Kurzy služby Azure Container Instances</span><span class="sxs-lookup"><span data-stu-id="34abf-135">Azure Container Instances tutorials</span></span>](./container-instances-tutorial-prepare-app.md)


<!-- LINKS -->
[app-github-repo]: https://github.com/Azure-Samples/aci-helloworld.git

<!-- IMAGES -->
[aci-app-browser]: ./media/container-instances-quickstart/aci-app-browser.png