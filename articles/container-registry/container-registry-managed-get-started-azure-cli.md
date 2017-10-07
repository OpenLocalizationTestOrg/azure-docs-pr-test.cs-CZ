---
title: "aaaCreate privátní registru kontejner Docker - rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Začínáme vytváření a správě privátního registrech kontejner Docker s hello 2.0 rozhraní příkazového řádku Azure"
services: container-registry
documentationcenter: 
author: neilpeterson
manager: timlt
editor: na
tags: 
keywords: 
ms.assetid: 29e20d75-bf39-4f7d-815f-a2e47209be7d
ms.service: container-registry
ms.devlang: azurecli
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/11/2017
ms.author: nepeters
ms.custom: na
ms.openlocfilehash: 2cadf42db0681a09c95486510f1e65c6f87c5280
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-managed-container-registry-using-hello-azure-cli"></a><span data-ttu-id="f22c2-103">Vytvoření spravované kontejneru registru pomocí hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="f22c2-103">Create a managed container registry using hello Azure CLI</span></span>

<span data-ttu-id="f22c2-104">Azure Container Registry je spravovaná služba registru kontejnerů Dockeru sloužící k ukládání privátních imagí kontejnerů Dockeru.</span><span class="sxs-lookup"><span data-stu-id="f22c2-104">Azure Container Registry is a managed Docker container registry service used for storing private Docker container images.</span></span> <span data-ttu-id="f22c2-105">Tato příručka podrobně popisuje vytváření spravované pomocí instance registru kontejner Azure hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="f22c2-105">This guide details creating a managed Azure Container Registry instance using hello Azure CLI.</span></span>

<span data-ttu-id="f22c2-106">Spravované registry kontejnerů Azure jsou ve verzi Preview a nejsou dostupné ve všech oblastech.</span><span class="sxs-lookup"><span data-stu-id="f22c2-106">Managed Azure container registries are in preview and not available in all regions.</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="f22c2-107">Pokud zvolíte tooinstall a místně pomocí hello rozhraní příkazového řádku, tento rychlý start vyžaduje, že používáte verzi rozhraní příkazového řádku Azure hello verze 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="f22c2-107">If you choose tooinstall and use hello CLI locally, this quickstart requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="f22c2-108">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="f22c2-108">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="f22c2-109">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="f22c2-109">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="create-a-resource-group"></a><span data-ttu-id="f22c2-110">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="f22c2-110">Create a resource group</span></span>

<span data-ttu-id="f22c2-111">Vytvořte skupinu prostředků s hello [vytvořit skupinu az](/cli/azure/group#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="f22c2-111">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="f22c2-112">Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="f22c2-112">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> 

<span data-ttu-id="f22c2-113">Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *westcentralus* umístění.</span><span class="sxs-lookup"><span data-stu-id="f22c2-113">hello following example creates a resource group named *myResourceGroup* in hello *westcentralus* location.</span></span>

```azurecli-interactive 
az group create --name myResourceGroup --location westcentralus
```

## <a name="create-a-container-registry"></a><span data-ttu-id="f22c2-114">Vytvoření registru kontejnerů</span><span class="sxs-lookup"><span data-stu-id="f22c2-114">Create a container registry</span></span>

<span data-ttu-id="f22c2-115">Vytvoření instance ACR pomocí hello [vytvořit az acr](/cli/azure/acr#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="f22c2-115">Create an ACR instance using hello [az acr create](/cli/azure/acr#create) command.</span></span>

> [!NOTE]
> <span data-ttu-id="f22c2-116">Při vytváření registru zadejte globálně jedinečný název domény nejvyšší úrovně obsahující pouze písmena a číslice.</span><span class="sxs-lookup"><span data-stu-id="f22c2-116">When you create a registry, specify a globally unique top-level domain name, containing only letters and numbers.</span></span>

 <span data-ttu-id="f22c2-117">Název registru Hello v příkladu hello je *myContainerRegistry1*, nahraďte vlastní jedinečný název.</span><span class="sxs-lookup"><span data-stu-id="f22c2-117">hello registry name in hello example is *myContainerRegistry1*, substitute a unique name of your own.</span></span>

```azurecli
az acr create --name myContainerRegistry1 --resource-group myResourceGroup --sku Managed_Standard
```

<span data-ttu-id="f22c2-118">Při vytváření hello registru výstup hello je podobné toohello následující:</span><span class="sxs-lookup"><span data-stu-id="f22c2-118">When hello registry is created, hello output is similar toohello following:</span></span>

```azurecli
{
  "adminUserEnabled": false,
  "creationDate": "2017-06-29T04:50:28.607134+00:00",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/myContainerRegistry1",
  "location": "westcentralus",
  "loginServer": "mycontainerregistry1.azurecr.io",
  "name": "myContainerRegistry1",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "name": "Managed_Standard",
    "tier": "Managed"
  },
  "storageAccount": null,
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

## <a name="log-in-tooacr-instance"></a><span data-ttu-id="f22c2-119">Přihlaste se tooACR instance</span><span class="sxs-lookup"><span data-stu-id="f22c2-119">Log in tooACR instance</span></span>

<span data-ttu-id="f22c2-120">Před vkládání a stahování kontejneru bitové kopie, musíte být přihlášení toohello ACR instance.</span><span class="sxs-lookup"><span data-stu-id="f22c2-120">Before pushing and pulling container images, you must log in toohello ACR instance.</span></span> <span data-ttu-id="f22c2-121">toodo Ano, použít hello [az acr přihlášení](/cli/azure/acr#login) příkaz.</span><span class="sxs-lookup"><span data-stu-id="f22c2-121">toodo so, use hello [az acr login](/cli/azure/acr#login) command.</span></span>

```azurecli-interactive
az acr login --name myAzureContainerRegistry1
```

<span data-ttu-id="f22c2-122">příkaz Hello vrátí zprávu byla úspěšná přihlášení po dokončení.</span><span class="sxs-lookup"><span data-stu-id="f22c2-122">hello command returns a 'Login Succeeded' message once completed.</span></span>

## <a name="use-azure-container-registry"></a><span data-ttu-id="f22c2-123">Použití služby Azure Container Registry</span><span class="sxs-lookup"><span data-stu-id="f22c2-123">Use Azure Container Registry</span></span>

### <a name="list-container-images"></a><span data-ttu-id="f22c2-124">Výpis imagí kontejnerů</span><span class="sxs-lookup"><span data-stu-id="f22c2-124">List container images</span></span>

<span data-ttu-id="f22c2-125">Použití hello `az acr` příkazy rozhraní příkazového řádku tooquery hello Image a značky v úložišti.</span><span class="sxs-lookup"><span data-stu-id="f22c2-125">Use hello `az acr` CLI commands tooquery hello images and tags in a repository.</span></span>

> [!NOTE]
> <span data-ttu-id="f22c2-126">Kontejner registru v současné době nepodporuje hello `docker search` tooquery příkaz pro bitové kopie a značky.</span><span class="sxs-lookup"><span data-stu-id="f22c2-126">Currently, Container Registry does not support hello `docker search` command tooquery for images and tags.</span></span>

### <a name="list-repositories"></a><span data-ttu-id="f22c2-127">Výpis úložišť</span><span class="sxs-lookup"><span data-stu-id="f22c2-127">List repositories</span></span>

<span data-ttu-id="f22c2-128">Hello následující příklad uvádí hello úložiště v registru, ve formátu JSON (JavaScript Object Notation):</span><span class="sxs-lookup"><span data-stu-id="f22c2-128">hello following example lists hello repositories in a registry, in JSON (JavaScript Object Notation) format:</span></span>

```azurecli
az acr repository list -n myContainerRegistry1 -o json
```

### <a name="list-tags"></a><span data-ttu-id="f22c2-129">Výpis značek</span><span class="sxs-lookup"><span data-stu-id="f22c2-129">List tags</span></span>

<span data-ttu-id="f22c2-130">Hello následující příklad vypíše značky hello na hello **ukázky/nginx** úložiště, ve formátu JSON:</span><span class="sxs-lookup"><span data-stu-id="f22c2-130">hello following example lists hello tags on hello **samples/nginx** repository, in JSON format:</span></span>

```azurecli
az acr repository show-tags -n myContainerRegistry1 --repository samples/nginx -o json
```

## <a name="next-steps"></a><span data-ttu-id="f22c2-131">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f22c2-131">Next steps</span></span>

<span data-ttu-id="f22c2-132">V této úvodní jste vytvořili spravované instance registru kontejner Azure pomocí hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="f22c2-132">In this quick start, you've created a managed Azure Container Registry instance using hello Azure CLI.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="f22c2-133">Push vaší první image pomocí příkazového řádku Dockeru hello</span><span class="sxs-lookup"><span data-stu-id="f22c2-133">Push your first image using hello Docker CLI</span></span>](container-registry-get-started-docker-cli.md)
