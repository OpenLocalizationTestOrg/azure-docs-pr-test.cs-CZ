---
title: "Kurz pro Azure instancí kontejnerů – Příprava registru kontejner Azure | Microsoft Docs"
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
ms.openlocfilehash: cc96ba9f5abd45a7503ba3327b30e1f809391384
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="deploy-and-use-azure-container-registry"></a><span data-ttu-id="80e43-104">Nasazení a používání Azure kontejneru registru</span><span class="sxs-lookup"><span data-stu-id="80e43-104">Deploy and use Azure Container Registry</span></span>

<span data-ttu-id="80e43-105">Toto je část dva kurzu třemi částmi.</span><span class="sxs-lookup"><span data-stu-id="80e43-105">This is part two of a three-part tutorial.</span></span> <span data-ttu-id="80e43-106">V [předchozího kroku](./container-instances-tutorial-prepare-app.md), bitovou kopii kontejner byl vytvořen pro jednoduché webové aplikace napsané v [Node.js](http://nodejs.org).</span><span class="sxs-lookup"><span data-stu-id="80e43-106">In the [previous step](./container-instances-tutorial-prepare-app.md), a container image was created for a simple web application written in [Node.js](http://nodejs.org).</span></span> <span data-ttu-id="80e43-107">V tomto kurzu se posune tuto bitovou kopii registru kontejner Azure.</span><span class="sxs-lookup"><span data-stu-id="80e43-107">In this tutorial, this image is pushed to an Azure Container Registry.</span></span> <span data-ttu-id="80e43-108">Pokud jste ještě nevytvořili bitovou kopii kontejneru, vrátit [kurzu 1 – Vytvoření kontejneru image](./container-instances-tutorial-prepare-app.md).</span><span class="sxs-lookup"><span data-stu-id="80e43-108">If you have not created the container image, return to [Tutorial 1 – Create container image](./container-instances-tutorial-prepare-app.md).</span></span> 

<span data-ttu-id="80e43-109">Registr kontejner Azure je založené na Azure, privátní registru, pro kontejner Docker bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="80e43-109">The Azure Container Registry is an Azure-based, private registry, for Docker container images.</span></span> <span data-ttu-id="80e43-110">Tento kurz vás provede nasazení instanci Azure Container registru a když zavedete bitovou kopii kontejneru.</span><span class="sxs-lookup"><span data-stu-id="80e43-110">This tutorial walks through deploying an Azure Container Registry instance, and pushing a container image to it.</span></span> <span data-ttu-id="80e43-111">Dokončit krokům patří:</span><span class="sxs-lookup"><span data-stu-id="80e43-111">Steps completed include:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="80e43-112">Nasazení instanci Azure Container registru</span><span class="sxs-lookup"><span data-stu-id="80e43-112">Deploying an Azure Container Registry instance</span></span>
> * <span data-ttu-id="80e43-113">Označování kontejneru image pro registru kontejner Azure</span><span class="sxs-lookup"><span data-stu-id="80e43-113">Tagging container image for Azure Container Registry</span></span>
> * <span data-ttu-id="80e43-114">Odesílání bitové kopie do registru kontejner Azure</span><span class="sxs-lookup"><span data-stu-id="80e43-114">Uploading image to Azure Container Registry</span></span>

<span data-ttu-id="80e43-115">V následujících kurzech můžete nasadit kontejner z vaší privátní registru pro kontejner instancí Azure.</span><span class="sxs-lookup"><span data-stu-id="80e43-115">In subsequent tutorials, you deploy the container from your private registry to Azure Container Instances.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="80e43-116">Než začnete</span><span class="sxs-lookup"><span data-stu-id="80e43-116">Before you begin</span></span>

<span data-ttu-id="80e43-117">Tento kurz vyžaduje, že používáte Azure CLI verze verze 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="80e43-117">This tutorial requires that you are running the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="80e43-118">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="80e43-118">Run `az --version` to find the version.</span></span> <span data-ttu-id="80e43-119">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="80e43-119">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span>

## <a name="deploy-azure-container-registry"></a><span data-ttu-id="80e43-120">Nasadit kontejner Azure registru</span><span class="sxs-lookup"><span data-stu-id="80e43-120">Deploy Azure Container Registry</span></span>

<span data-ttu-id="80e43-121">Pokud nasazujete registru kontejneru služby Azure, musíte nejprve skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="80e43-121">When deploying an Azure Container Registry, you first need a resource group.</span></span> <span data-ttu-id="80e43-122">Skupinu prostředků Azure je logické kolekce do Azure, které jsou nasazené a spravované prostředky.</span><span class="sxs-lookup"><span data-stu-id="80e43-122">An Azure resource group is a logical collection into which Azure resources are deployed and managed.</span></span>

<span data-ttu-id="80e43-123">Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="80e43-123">Create a resource group with the [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="80e43-124">V tomto příkladu skupinu prostředků s názvem *myResourceGroup* je vytvořen v *eastus* oblast.</span><span class="sxs-lookup"><span data-stu-id="80e43-124">In this example, a resource group named *myResourceGroup* is created in the *eastus* region.</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="80e43-125">Vytvoření kontejneru Azure registr s využitím [az acr vytvořit](/cli/azure/acr#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="80e43-125">Create an Azure Container registry with the [az acr create](/cli/azure/acr#create) command.</span></span> <span data-ttu-id="80e43-126">Název kontejneru registru **musí být jedinečné**.</span><span class="sxs-lookup"><span data-stu-id="80e43-126">The name of a Container Registry **must be unique**.</span></span> <span data-ttu-id="80e43-127">V následujícím příkladu použijeme název *mycontainerregistry082*.</span><span class="sxs-lookup"><span data-stu-id="80e43-127">In the following example, we use the name *mycontainerregistry082*.</span></span>

```azurecli
az acr create --resource-group myResourceGroup --name mycontainerregistry082 --sku Basic --admin-enabled true
```

<span data-ttu-id="80e43-128">Po celý zbytek v tomto kurzu používáme `<acrname>` jako zástupný symbol pro název kontejneru registru, který jste zvolili.</span><span class="sxs-lookup"><span data-stu-id="80e43-128">Throughout the rest of this tutorial, we use `<acrname>` as a placeholder for the container registry name that you chose.</span></span>

## <a name="container-registry-login"></a><span data-ttu-id="80e43-129">Kontejner registru přihlášení</span><span class="sxs-lookup"><span data-stu-id="80e43-129">Container registry login</span></span>

<span data-ttu-id="80e43-130">Musíte se přihlásit k vaší instanci ACR před odesláním bitové kopie do ní.</span><span class="sxs-lookup"><span data-stu-id="80e43-130">You must log in to your ACR instance before pushing images to it.</span></span> <span data-ttu-id="80e43-131">Použití [az acr přihlášení](https://docs.microsoft.com/en-us/cli/azure/acr#login) příkaz k dokončení operace.</span><span class="sxs-lookup"><span data-stu-id="80e43-131">Use the [az acr login](https://docs.microsoft.com/en-us/cli/azure/acr#login) command to complete the operation.</span></span> <span data-ttu-id="80e43-132">Je třeba zadat jedinečný název zadané registru kontejneru v okamžiku vytvoření.</span><span class="sxs-lookup"><span data-stu-id="80e43-132">You need to provide the unique name given to the container registry when it was created.</span></span>

```azurecli
az acr login --name <acrName>
```

<span data-ttu-id="80e43-133">Příkaz vrátí zprávu, byla úspěšná přihlášení po dokončení.</span><span class="sxs-lookup"><span data-stu-id="80e43-133">The command returns a 'Login Succeeded’ message once completed.</span></span>

## <a name="tag-container-image"></a><span data-ttu-id="80e43-134">Obrázek značky kontejneru</span><span class="sxs-lookup"><span data-stu-id="80e43-134">Tag container image</span></span>

<span data-ttu-id="80e43-135">K nasazení bitové kopie kontejneru z privátní registru, musí být označené bitovou kopii `loginServer` název registru.</span><span class="sxs-lookup"><span data-stu-id="80e43-135">To deploy a container image from a private registry, the image needs to be tagged with the `loginServer` name of the registry.</span></span>

<span data-ttu-id="80e43-136">Chcete-li zobrazit seznam aktuální bitové kopie, použijte `docker images` příkaz.</span><span class="sxs-lookup"><span data-stu-id="80e43-136">To see a list of current images, use the `docker images` command.</span></span>

```bash
docker images
```

<span data-ttu-id="80e43-137">Výstup:</span><span class="sxs-lookup"><span data-stu-id="80e43-137">Output:</span></span>

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED              SIZE
aci-tutorial-app             latest              5c745774dfa9        39 seconds ago       68.1 MB
```

<span data-ttu-id="80e43-138">Získat název loginServer, spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="80e43-138">To get the loginServer name, run the following command.</span></span>

```azurecli
az acr show --name <acrName> --query loginServer --output table
```

<span data-ttu-id="80e43-139">Značka *aci – kurz aplikace* bitovou kopii s loginServer registru kontejneru.</span><span class="sxs-lookup"><span data-stu-id="80e43-139">Tag the *aci-tutorial-app* image with the loginServer of the container registry.</span></span> <span data-ttu-id="80e43-140">Navíc přidat `:v1` na konec název bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="80e43-140">Also, add `:v1` to the end of the image name.</span></span> <span data-ttu-id="80e43-141">Tato značka označuje číslo verze bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="80e43-141">This tag indicates the image version number.</span></span>

```bash
docker tag aci-tutorial-app <acrLoginServer>/aci-tutorial-app:v1
```

<span data-ttu-id="80e43-142">Jakmile příznakem, spustit `docker images` ověření operaci.</span><span class="sxs-lookup"><span data-stu-id="80e43-142">Once tagged, run `docker images` to verify the operation.</span></span>

```bash
docker images
```

<span data-ttu-id="80e43-143">Výstup:</span><span class="sxs-lookup"><span data-stu-id="80e43-143">Output:</span></span>

```bash
REPOSITORY                                                TAG                 IMAGE ID            CREATED             SIZE
aci-tutorial-app                                          latest              5c745774dfa9        39 seconds ago      68.1 MB
mycontainerregistry082.azurecr.io/aci-tutorial-app        v1                  a9dace4e1a17        7 minutes ago       68.1 MB
```

## <a name="push-image-to-azure-container-registry"></a><span data-ttu-id="80e43-144">Push bitové kopie do registru kontejner Azure</span><span class="sxs-lookup"><span data-stu-id="80e43-144">Push image to Azure Container Registry</span></span>

<span data-ttu-id="80e43-145">Push *aci – kurz aplikace* bitovou kopii do registru.</span><span class="sxs-lookup"><span data-stu-id="80e43-145">Push the *aci-tutorial-app* image to the registry.</span></span>

<span data-ttu-id="80e43-146">Pomocí následujícího příkladu, nahraďte název kontejneru registru loginServer loginServer ze svého prostředí.</span><span class="sxs-lookup"><span data-stu-id="80e43-146">Using the following example, replace the container registry loginServer name with the loginServer from your environment.</span></span>

```bash
docker push <acrLoginServer>/aci-tutorial-app:v1
```

## <a name="list-images-in-azure-container-registry"></a><span data-ttu-id="80e43-147">Seznam obrázků v registru kontejner Azure</span><span class="sxs-lookup"><span data-stu-id="80e43-147">List images in Azure Container Registry</span></span>

<span data-ttu-id="80e43-148">K zobrazení seznamu bitové kopie, které se nabídne do vašeho kontejneru Azure registru uživatele [az acr úložiště seznamu](/cli/azure/acr/repository#list) příkaz.</span><span class="sxs-lookup"><span data-stu-id="80e43-148">To return a list of images that have been pushed to your Azure Container registry, user the [az acr repository list](/cli/azure/acr/repository#list) command.</span></span> <span data-ttu-id="80e43-149">Aktualizujte příkaz s názvem registru kontejneru.</span><span class="sxs-lookup"><span data-stu-id="80e43-149">Update the command with the container registry name.</span></span>

```azurecli
az acr repository list --name <acrName> --output table
```

<span data-ttu-id="80e43-150">Výstup:</span><span class="sxs-lookup"><span data-stu-id="80e43-150">Output:</span></span>

```azurecli
Result
----------------
aci-tutorial-app
```

<span data-ttu-id="80e43-151">A pak najdete v části značky pro konkrétní image, pomocí [az acr úložiště zobrazit značky](/cli/azure/acr/repository#show-tags) příkaz.</span><span class="sxs-lookup"><span data-stu-id="80e43-151">And then to see the tags for a specific image, use the [az acr repository show-tags](/cli/azure/acr/repository#show-tags) command.</span></span>

```azurecli
az acr repository show-tags --name <acrName> --repository aci-tutorial-app --output table
```

<span data-ttu-id="80e43-152">Výstup:</span><span class="sxs-lookup"><span data-stu-id="80e43-152">Output:</span></span>

```azurecli
Result
--------
v1
```

## <a name="next-steps"></a><span data-ttu-id="80e43-153">Další kroky</span><span class="sxs-lookup"><span data-stu-id="80e43-153">Next steps</span></span>

<span data-ttu-id="80e43-154">V tomto kurzu registru kontejner Azure připravené pro použití s instancemi Azure kontejneru a bitovou kopii kontejneru se instaluje.</span><span class="sxs-lookup"><span data-stu-id="80e43-154">In this tutorial, an Azure Container Registry was prepared for use with Azure Container Instances, and the container image was pushed.</span></span> <span data-ttu-id="80e43-155">Dokončili jste následující kroky:</span><span class="sxs-lookup"><span data-stu-id="80e43-155">The following steps were completed:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="80e43-156">Nasazení instanci Azure Container registru</span><span class="sxs-lookup"><span data-stu-id="80e43-156">Deploying an Azure Container Registry instance</span></span>
> * <span data-ttu-id="80e43-157">Označování kontejneru image pro registru kontejner Azure</span><span class="sxs-lookup"><span data-stu-id="80e43-157">Tagging container image for Azure Container Registry</span></span>
> * <span data-ttu-id="80e43-158">Odesílání bitové kopie do registru kontejner Azure</span><span class="sxs-lookup"><span data-stu-id="80e43-158">Uploading image to Azure Container Registry</span></span>

<span data-ttu-id="80e43-159">Přechodu na další kurzu se dozvíte o nasazení do Azure pomocí Azure kontejner instancí kontejneru.</span><span class="sxs-lookup"><span data-stu-id="80e43-159">Advance to the next tutorial to learn about deploying the container to Azure using Azure Container Instances.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="80e43-160">Kontejnery můžete nasadit do Azure kontejner instancí</span><span class="sxs-lookup"><span data-stu-id="80e43-160">Deploy containers to Azure Container Instances</span></span>](./container-instances-tutorial-deploy-app.md)
