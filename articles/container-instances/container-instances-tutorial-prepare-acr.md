---
title: "kurz instancí kontejnerů aaaAzure – Příprava registru kontejner Azure | Microsoft Docs"
description: "Kurz pro Azure instancí kontejnerů – Příprava registru kontejner Azure"
services: container-instances
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, Kontejnery, mikroslužby, Kubernetes, DC/OS, Azure"
ms.assetid: 
ms.service: container-instances
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 2525626125740c3c861fad36aad207d0b587ff54
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-use-azure-container-registry"></a><span data-ttu-id="15eb9-104">Nasazení a používání Azure kontejneru registru</span><span class="sxs-lookup"><span data-stu-id="15eb9-104">Deploy and use Azure Container Registry</span></span>

<span data-ttu-id="15eb9-105">Toto je část dva kurzu třemi částmi.</span><span class="sxs-lookup"><span data-stu-id="15eb9-105">This is part two of a three-part tutorial.</span></span> <span data-ttu-id="15eb9-106">V hello [předchozího kroku](./container-instances-tutorial-prepare-app.md), bitovou kopii kontejner byl vytvořen pro jednoduché webové aplikace napsané v [Node.js](http://nodejs.org).</span><span class="sxs-lookup"><span data-stu-id="15eb9-106">In hello [previous step](./container-instances-tutorial-prepare-app.md), a container image was created for a simple web application written in [Node.js](http://nodejs.org).</span></span> <span data-ttu-id="15eb9-107">V tomto kurzu se posune tuto bitovou kopii tooan registru kontejner Azure.</span><span class="sxs-lookup"><span data-stu-id="15eb9-107">In this tutorial, this image is pushed tooan Azure Container Registry.</span></span> <span data-ttu-id="15eb9-108">Pokud jste dosud nevytvořili hello kontejneru image, vrátí příliš[kurzu 1 – Vytvoření kontejneru image](./container-instances-tutorial-prepare-app.md).</span><span class="sxs-lookup"><span data-stu-id="15eb9-108">If you have not created hello container image, return too[Tutorial 1 – Create container image](./container-instances-tutorial-prepare-app.md).</span></span> 

<span data-ttu-id="15eb9-109">Hello registru kontejner Azure je založené na Azure, privátní registru, pro kontejner Docker bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="15eb9-109">hello Azure Container Registry is an Azure-based, private registry, for Docker container images.</span></span> <span data-ttu-id="15eb9-110">Tento kurz vás provede nasazení instanci Azure Container registru a předání tooit bitové kopie kontejneru.</span><span class="sxs-lookup"><span data-stu-id="15eb9-110">This tutorial walks through deploying an Azure Container Registry instance, and pushing a container image tooit.</span></span> <span data-ttu-id="15eb9-111">Dokončit krokům patří:</span><span class="sxs-lookup"><span data-stu-id="15eb9-111">Steps completed include:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="15eb9-112">Nasazení instanci Azure Container registru</span><span class="sxs-lookup"><span data-stu-id="15eb9-112">Deploying an Azure Container Registry instance</span></span>
> * <span data-ttu-id="15eb9-113">Označování kontejneru image pro registru kontejner Azure</span><span class="sxs-lookup"><span data-stu-id="15eb9-113">Tagging container image for Azure Container Registry</span></span>
> * <span data-ttu-id="15eb9-114">Odesílání bitové kopie tooAzure registru kontejneru</span><span class="sxs-lookup"><span data-stu-id="15eb9-114">Uploading image tooAzure Container Registry</span></span>

<span data-ttu-id="15eb9-115">V následujících kurzech nasadit hello kontejneru z vaší privátní registru tooAzure instancí kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="15eb9-115">In subsequent tutorials, you deploy hello container from your private registry tooAzure Container Instances.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="15eb9-116">Než začnete</span><span class="sxs-lookup"><span data-stu-id="15eb9-116">Before you begin</span></span>

<span data-ttu-id="15eb9-117">Tento kurz vyžaduje, že používáte verzi rozhraní příkazového řádku Azure hello verze 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="15eb9-117">This tutorial requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="15eb9-118">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="15eb9-118">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="15eb9-119">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="15eb9-119">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span>

## <a name="deploy-azure-container-registry"></a><span data-ttu-id="15eb9-120">Nasadit kontejner Azure registru</span><span class="sxs-lookup"><span data-stu-id="15eb9-120">Deploy Azure Container Registry</span></span>

<span data-ttu-id="15eb9-121">Pokud nasazujete registru kontejneru služby Azure, musíte nejprve skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="15eb9-121">When deploying an Azure Container Registry, you first need a resource group.</span></span> <span data-ttu-id="15eb9-122">Skupinu prostředků Azure je logické kolekce do Azure, které jsou nasazené a spravované prostředky.</span><span class="sxs-lookup"><span data-stu-id="15eb9-122">An Azure resource group is a logical collection into which Azure resources are deployed and managed.</span></span>

<span data-ttu-id="15eb9-123">Vytvořte skupinu prostředků s hello [vytvořit skupinu az](/cli/azure/group#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="15eb9-123">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="15eb9-124">V tomto příkladu skupinu prostředků s názvem *myResourceGroup* je vytvořen v hello *eastus* oblast.</span><span class="sxs-lookup"><span data-stu-id="15eb9-124">In this example, a resource group named *myResourceGroup* is created in hello *eastus* region.</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="15eb9-125">Vytvoření registru kontejneru Azure s hello [vytvořit az acr](/cli/azure/acr#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="15eb9-125">Create an Azure Container registry with hello [az acr create](/cli/azure/acr#create) command.</span></span> <span data-ttu-id="15eb9-126">název kontejneru registru Hello **musí být jedinečné**.</span><span class="sxs-lookup"><span data-stu-id="15eb9-126">hello name of a Container Registry **must be unique**.</span></span> <span data-ttu-id="15eb9-127">V následující ukázka hello, použijeme hello název *mycontainerregistry082*.</span><span class="sxs-lookup"><span data-stu-id="15eb9-127">In hello following example, we use hello name *mycontainerregistry082*.</span></span>

```azurecli
az acr create --resource-group myResourceGroup --name mycontainerregistry082 --sku Basic --admin-enabled true
```

<span data-ttu-id="15eb9-128">V rámci hello zbytek v tomto kurzu používáme `<acrname>` jako zástupný symbol pro název registru hello kontejneru, který jste si zvolili.</span><span class="sxs-lookup"><span data-stu-id="15eb9-128">Throughout hello rest of this tutorial, we use `<acrname>` as a placeholder for hello container registry name that you chose.</span></span>

## <a name="container-registry-login"></a><span data-ttu-id="15eb9-129">Kontejner registru přihlášení</span><span class="sxs-lookup"><span data-stu-id="15eb9-129">Container registry login</span></span>

<span data-ttu-id="15eb9-130">Musíte se přihlásit v instanci ACR tooyour před odesláním tooit bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="15eb9-130">You must log in tooyour ACR instance before pushing images tooit.</span></span> <span data-ttu-id="15eb9-131">Použití hello [az acr přihlášení](https://docs.microsoft.com/en-us/cli/azure/acr#login) příkaz toocomplete hello operaci.</span><span class="sxs-lookup"><span data-stu-id="15eb9-131">Use hello [az acr login](https://docs.microsoft.com/en-us/cli/azure/acr#login) command toocomplete hello operation.</span></span> <span data-ttu-id="15eb9-132">Je nutné tooprovide hello jedinečný název zadané toohello kontejneru registru, v okamžiku vytvoření.</span><span class="sxs-lookup"><span data-stu-id="15eb9-132">You need tooprovide hello unique name given toohello container registry when it was created.</span></span>

```azurecli
az acr login --name <acrName>
```

<span data-ttu-id="15eb9-133">příkaz Hello vrátí zprávu byla úspěšná přihlášení po dokončení.</span><span class="sxs-lookup"><span data-stu-id="15eb9-133">hello command returns a 'Login Succeeded’ message once completed.</span></span>

## <a name="tag-container-image"></a><span data-ttu-id="15eb9-134">Obrázek značky kontejneru</span><span class="sxs-lookup"><span data-stu-id="15eb9-134">Tag container image</span></span>

<span data-ttu-id="15eb9-135">toodeploy kontejneru image z privátní registru hello image musí toobe označené hello `loginServer` název registru hello.</span><span class="sxs-lookup"><span data-stu-id="15eb9-135">toodeploy a container image from a private registry, hello image needs toobe tagged with hello `loginServer` name of hello registry.</span></span>

<span data-ttu-id="15eb9-136">toosee seznam aktuální Image, použijte hello `docker images` příkaz.</span><span class="sxs-lookup"><span data-stu-id="15eb9-136">toosee a list of current images, use hello `docker images` command.</span></span>

```bash
docker images
```

<span data-ttu-id="15eb9-137">Výstup:</span><span class="sxs-lookup"><span data-stu-id="15eb9-137">Output:</span></span>

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED              SIZE
aci-tutorial-app             latest              5c745774dfa9        39 seconds ago       68.1 MB
```

<span data-ttu-id="15eb9-138">tooget hello loginServer název, spusťte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="15eb9-138">tooget hello loginServer name, run hello following command.</span></span>

```azurecli
az acr show --name <acrName> --query loginServer --output table
```

<span data-ttu-id="15eb9-139">Značka hello *aci – kurz aplikace* bitovou kopii s loginServer hello hello kontejneru registru.</span><span class="sxs-lookup"><span data-stu-id="15eb9-139">Tag hello *aci-tutorial-app* image with hello loginServer of hello container registry.</span></span> <span data-ttu-id="15eb9-140">Navíc přidat `:v1` toohello konec hello název bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="15eb9-140">Also, add `:v1` toohello end of hello image name.</span></span> <span data-ttu-id="15eb9-141">Tato značka označuje číslo verze hello bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="15eb9-141">This tag indicates hello image version number.</span></span>

```bash
docker tag aci-tutorial-app <acrLoginServer>/aci-tutorial-app:v1
```

<span data-ttu-id="15eb9-142">Jakmile příznakem, spustit `docker images` tooverify hello operaci.</span><span class="sxs-lookup"><span data-stu-id="15eb9-142">Once tagged, run `docker images` tooverify hello operation.</span></span>

```bash
docker images
```

<span data-ttu-id="15eb9-143">Výstup:</span><span class="sxs-lookup"><span data-stu-id="15eb9-143">Output:</span></span>

```bash
REPOSITORY                                                TAG                 IMAGE ID            CREATED             SIZE
aci-tutorial-app                                          latest              5c745774dfa9        39 seconds ago      68.1 MB
mycontainerregistry082.azurecr.io/aci-tutorial-app        v1                  a9dace4e1a17        7 minutes ago       68.1 MB
```

## <a name="push-image-tooazure-container-registry"></a><span data-ttu-id="15eb9-144">Push image tooAzure registru kontejneru</span><span class="sxs-lookup"><span data-stu-id="15eb9-144">Push image tooAzure Container Registry</span></span>

<span data-ttu-id="15eb9-145">Push hello *aci – kurz aplikace* registru toohello bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="15eb9-145">Push hello *aci-tutorial-app* image toohello registry.</span></span>

<span data-ttu-id="15eb9-146">Pomocí následující ukázka hello, nahraďte název loginServer registru kontejneru hello hello loginServer ze svého prostředí.</span><span class="sxs-lookup"><span data-stu-id="15eb9-146">Using hello following example, replace hello container registry loginServer name with hello loginServer from your environment.</span></span>

```bash
docker push <acrLoginServer>/aci-tutorial-app:v1
```

## <a name="list-images-in-azure-container-registry"></a><span data-ttu-id="15eb9-147">Seznam obrázků v registru kontejner Azure</span><span class="sxs-lookup"><span data-stu-id="15eb9-147">List images in Azure Container Registry</span></span>

<span data-ttu-id="15eb9-148">tooreturn seznam bitové kopie, které se nabídne tooyour kontejneru Azure registru, uživatel hello [az acr úložiště seznamu](/cli/azure/acr/repository#list) příkaz.</span><span class="sxs-lookup"><span data-stu-id="15eb9-148">tooreturn a list of images that have been pushed tooyour Azure Container registry, user hello [az acr repository list](/cli/azure/acr/repository#list) command.</span></span> <span data-ttu-id="15eb9-149">Aktualizujte hello příkaz s názvem registru hello kontejneru.</span><span class="sxs-lookup"><span data-stu-id="15eb9-149">Update hello command with hello container registry name.</span></span>

```azurecli
az acr repository list --name <acrName> --output table
```

<span data-ttu-id="15eb9-150">Výstup:</span><span class="sxs-lookup"><span data-stu-id="15eb9-150">Output:</span></span>

```azurecli
Result
----------------
aci-tutorial-app
```

<span data-ttu-id="15eb9-151">A pak toosee hello značky pro konkrétní image, pomocí hello [az acr úložiště zobrazit značky](/cli/azure/acr/repository#show-tags) příkaz.</span><span class="sxs-lookup"><span data-stu-id="15eb9-151">And then toosee hello tags for a specific image, use hello [az acr repository show-tags](/cli/azure/acr/repository#show-tags) command.</span></span>

```azurecli
az acr repository show-tags --name <acrName> --repository aci-tutorial-app --output table
```

<span data-ttu-id="15eb9-152">Výstup:</span><span class="sxs-lookup"><span data-stu-id="15eb9-152">Output:</span></span>

```azurecli
Result
--------
v1
```

## <a name="next-steps"></a><span data-ttu-id="15eb9-153">Další kroky</span><span class="sxs-lookup"><span data-stu-id="15eb9-153">Next steps</span></span>

<span data-ttu-id="15eb9-154">V tomto kurzu registru kontejner Azure připravené pro použití s instancemi Azure kontejneru a byla posunuta hello kontejneru image.</span><span class="sxs-lookup"><span data-stu-id="15eb9-154">In this tutorial, an Azure Container Registry was prepared for use with Azure Container Instances, and hello container image was pushed.</span></span> <span data-ttu-id="15eb9-155">byly dokončeny Hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="15eb9-155">hello following steps were completed:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="15eb9-156">Nasazení instanci Azure Container registru</span><span class="sxs-lookup"><span data-stu-id="15eb9-156">Deploying an Azure Container Registry instance</span></span>
> * <span data-ttu-id="15eb9-157">Označování kontejneru image pro registru kontejner Azure</span><span class="sxs-lookup"><span data-stu-id="15eb9-157">Tagging container image for Azure Container Registry</span></span>
> * <span data-ttu-id="15eb9-158">Odesílání bitové kopie tooAzure registru kontejneru</span><span class="sxs-lookup"><span data-stu-id="15eb9-158">Uploading image tooAzure Container Registry</span></span>

<span data-ttu-id="15eb9-159">Posunutí další kurz toolearn toohello o nasazení tooAzure kontejneru hello pomocí Azure kontejner instancí.</span><span class="sxs-lookup"><span data-stu-id="15eb9-159">Advance toohello next tutorial toolearn about deploying hello container tooAzure using Azure Container Instances.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="15eb9-160">Nasazení kontejnerů tooAzure instancí kontejnerů</span><span class="sxs-lookup"><span data-stu-id="15eb9-160">Deploy containers tooAzure Container Instances</span></span>](./container-instances-tutorial-deploy-app.md)
