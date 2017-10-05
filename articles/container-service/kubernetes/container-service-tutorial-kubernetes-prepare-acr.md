---
title: "Kurz pro Azure Container Service – Příprava ACR | Microsoft Docs"
description: "Kurz pro Azure Container Service – Příprava ACR"
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, Kontejnery, mikroslužby, Kubernetes, DC/OS, Azure"
ms.assetid: 
ms.service: container-service
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/21/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 3e1f7617bf2fc52ee4c15598f51a46276f4dc57d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="deploy-and-use-azure-container-registry"></a><span data-ttu-id="a0a4a-104">Nasazení a používání Azure kontejneru registru</span><span class="sxs-lookup"><span data-stu-id="a0a4a-104">Deploy and use Azure Container Registry</span></span>

<span data-ttu-id="a0a4a-105">Azure kontejneru registru (ACR) je založené na Azure, privátní registru, pro kontejner Docker bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="a0a4a-105">Azure Container Registry (ACR) is an Azure-based, private registry, for Docker container images.</span></span> <span data-ttu-id="a0a4a-106">Tento kurz, část 7, obě provede nasazení instanci Azure Container registru a když zavedete bitovou kopii kontejneru.</span><span class="sxs-lookup"><span data-stu-id="a0a4a-106">This tutorial, part two of seven, walks through deploying an Azure Container Registry instance, and pushing a container image to it.</span></span> <span data-ttu-id="a0a4a-107">Dokončit krokům patří:</span><span class="sxs-lookup"><span data-stu-id="a0a4a-107">Steps completed include:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a0a4a-108">Nasazení instanci Azure Container registru (ACR)</span><span class="sxs-lookup"><span data-stu-id="a0a4a-108">Deploying an Azure Container Registry (ACR) instance</span></span>
> * <span data-ttu-id="a0a4a-109">Označování bitovou kopii kontejner pro ACR</span><span class="sxs-lookup"><span data-stu-id="a0a4a-109">Tagging a container image for ACR</span></span>
> * <span data-ttu-id="a0a4a-110">Nahrávání bitovou kopii do ACR</span><span class="sxs-lookup"><span data-stu-id="a0a4a-110">Uploading the image to ACR</span></span>

<span data-ttu-id="a0a4a-111">V následujících kurzech této instance ACR je integrovaná do clusteru služby Azure Container Service Kubernetes pro zabezpečené spouštění kontejneru bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="a0a4a-111">In subsequent tutorials, this ACR instance is integrated with an Azure Container Service Kubernetes cluster, for securely running container images.</span></span> 

## <a name="before-you-begin"></a><span data-ttu-id="a0a4a-112">Než začnete</span><span class="sxs-lookup"><span data-stu-id="a0a4a-112">Before you begin</span></span>

<span data-ttu-id="a0a4a-113">V [předchozí kurzu](./container-service-tutorial-kubernetes-prepare-app.md), bitovou kopii kontejner byl vytvořen pro jednoduchou aplikaci Azure hlasování.</span><span class="sxs-lookup"><span data-stu-id="a0a4a-113">In the [previous tutorial](./container-service-tutorial-kubernetes-prepare-app.md), a container image was created for a simple Azure Voting application.</span></span> <span data-ttu-id="a0a4a-114">V tomto kurzu se posune tuto bitovou kopii registru kontejner Azure.</span><span class="sxs-lookup"><span data-stu-id="a0a4a-114">In this tutorial, this image is pushed to an Azure Container Registry.</span></span> <span data-ttu-id="a0a4a-115">Pokud jste ještě nevytvořili obrázek aplikace Azure hlasování, vrátit [kurzu 1 – Vytvoření kontejneru image](./container-service-tutorial-kubernetes-prepare-app.md).</span><span class="sxs-lookup"><span data-stu-id="a0a4a-115">If you have not created the Azure Voting app image, return to [Tutorial 1 – Create container images](./container-service-tutorial-kubernetes-prepare-app.md).</span></span> <span data-ttu-id="a0a4a-116">Alternativně kroky podrobné zde pracují se žádný obrázek kontejneru.</span><span class="sxs-lookup"><span data-stu-id="a0a4a-116">Alternatively, the steps detailed here work with any container image.</span></span>

<span data-ttu-id="a0a4a-117">Tento kurz vyžaduje, že používáte Azure CLI verze verze 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="a0a4a-117">This tutorial requires that you are running the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="a0a4a-118">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="a0a4a-118">Run `az --version` to find the version.</span></span> <span data-ttu-id="a0a4a-119">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="a0a4a-119">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="deploy-azure-container-registry"></a><span data-ttu-id="a0a4a-120">Nasadit kontejner Azure registru</span><span class="sxs-lookup"><span data-stu-id="a0a4a-120">Deploy Azure Container Registry</span></span>

<span data-ttu-id="a0a4a-121">Pokud nasazujete registru kontejneru služby Azure, musíte nejprve skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="a0a4a-121">When deploying an Azure Container Registry, you first need a resource group.</span></span> <span data-ttu-id="a0a4a-122">Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="a0a4a-122">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span>

<span data-ttu-id="a0a4a-123">Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="a0a4a-123">Create a resource group with the [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="a0a4a-124">V tomto příkladu skupinu prostředků s názvem *myResourceGroup* je vytvořen v *westeurope* oblast.</span><span class="sxs-lookup"><span data-stu-id="a0a4a-124">In this example, a resource group named *myResourceGroup* is created in the *westeurope* region.</span></span>

```azurecli
az group create --name myResourceGroup --location westeurope
```

<span data-ttu-id="a0a4a-125">Vytvoření kontejneru Azure registr s využitím [az acr vytvořit](/cli/azure/acr#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="a0a4a-125">Create an Azure Container registry with the [az acr create](/cli/azure/acr#create) command.</span></span> <span data-ttu-id="a0a4a-126">Název kontejneru registru **musí být jedinečné**.</span><span class="sxs-lookup"><span data-stu-id="a0a4a-126">The name of a Container Registry **must be unique**.</span></span>

```azurecli
az acr create --resource-group myResourceGroup --name <acrName> --sku Basic --admin-enabled true
```

<span data-ttu-id="a0a4a-127">Po celý zbytek v tomto kurzu používáme "acrname" jako zástupný symbol pro název kontejneru registru, který jste si zvolili.</span><span class="sxs-lookup"><span data-stu-id="a0a4a-127">Throughout the rest of this tutorial, we use "acrname" as a placeholder for the container registry name that you chose.</span></span>

## <a name="container-registry-login"></a><span data-ttu-id="a0a4a-128">Kontejner registru přihlášení</span><span class="sxs-lookup"><span data-stu-id="a0a4a-128">Container registry login</span></span>

<span data-ttu-id="a0a4a-129">Musíte se přihlásit k vaší instanci ACR před odesláním bitové kopie do ní.</span><span class="sxs-lookup"><span data-stu-id="a0a4a-129">You must log in to your ACR instance before pushing images to it.</span></span> <span data-ttu-id="a0a4a-130">Použití [az acr přihlášení](https://docs.microsoft.com/en-us/cli/azure/acr#login) příkaz k dokončení operace.</span><span class="sxs-lookup"><span data-stu-id="a0a4a-130">Use the [az acr login](https://docs.microsoft.com/en-us/cli/azure/acr#login) command to complete the operation.</span></span> <span data-ttu-id="a0a4a-131">Je třeba zadat jedinečný název zadané registru kontejneru v okamžiku vytvoření.</span><span class="sxs-lookup"><span data-stu-id="a0a4a-131">You need to provide the unique name given to the container registry when it was created.</span></span>

```azurecli
az acr login --name <acrName>
```

<span data-ttu-id="a0a4a-132">Příkaz vrátí zprávu, byla úspěšná přihlášení po dokončení.</span><span class="sxs-lookup"><span data-stu-id="a0a4a-132">The command returns a 'Login Succeeded’ message once completed.</span></span>

## <a name="tag-container-images"></a><span data-ttu-id="a0a4a-133">Značka kontejneru obrázků</span><span class="sxs-lookup"><span data-stu-id="a0a4a-133">Tag container images</span></span>

<span data-ttu-id="a0a4a-134">Každé bitové kopie kontejneru musí být označené loginServer název registru.</span><span class="sxs-lookup"><span data-stu-id="a0a4a-134">Each container image needs to be tagged with the loginServer name of the registry.</span></span> <span data-ttu-id="a0a4a-135">Tato značka se používá pro směrování při nabízení kontejneru bitové kopie do registru bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="a0a4a-135">This tag is used for routing when pushing container images to an image registry.</span></span>

<span data-ttu-id="a0a4a-136">Chcete-li zobrazit seznam aktuální bitové kopie, použijte [imagí dockeru](https://docs.docker.com/engine/reference/commandline/images/) příkaz.</span><span class="sxs-lookup"><span data-stu-id="a0a4a-136">To see a list of current images, use the [docker images](https://docs.docker.com/engine/reference/commandline/images/) command.</span></span>

```bash
docker images
```

<span data-ttu-id="a0a4a-137">Výstup:</span><span class="sxs-lookup"><span data-stu-id="a0a4a-137">Output:</span></span>

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED             SIZE
azure-vote-front             latest              4675398c9172        13 minutes ago      694MB
redis                        latest              a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask   flask               788ca94b2313        9 months ago        694MB
```

<span data-ttu-id="a0a4a-138">Získat název loginServer, spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="a0a4a-138">To get the loginServer name, run the following command.</span></span>

```azurecli
az acr show --name <acrName> --query loginServer --output table
```

<span data-ttu-id="a0a4a-139">Nyní, značky *azure hlas front* bitovou kopii s loginServer registru kontejneru.</span><span class="sxs-lookup"><span data-stu-id="a0a4a-139">Now, tag the *azure-vote-front* image with the loginServer of the container registry.</span></span> <span data-ttu-id="a0a4a-140">Navíc přidat `:redis-v1` na konec název bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="a0a4a-140">Also, add `:redis-v1` to the end of the image name.</span></span> <span data-ttu-id="a0a4a-141">Tato značka označuje verzi bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="a0a4a-141">This tag indicates the image version.</span></span>

```bash
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:redis-v1
```

<span data-ttu-id="a0a4a-142">Jakmile příznakem, spustit [imagí dockeru] (https://docs.docker.com/engine/reference/commandline/images/) k ověření operaci.</span><span class="sxs-lookup"><span data-stu-id="a0a4a-142">Once tagged, run [docker images] (https://docs.docker.com/engine/reference/commandline/images/) to verify the operation.</span></span>

```bash
docker images
```

<span data-ttu-id="a0a4a-143">Výstup:</span><span class="sxs-lookup"><span data-stu-id="a0a4a-143">Output:</span></span>

```bash
REPOSITORY                                           TAG                 IMAGE ID            CREATED             SIZE
azure-vote-front                                     latest              eaf2b9c57e5e        8 minutes ago       716 MB
mycontainerregistry082.azurecr.io/azure-vote-front   redis-v1            eaf2b9c57e5e        8 minutes ago       716 MB
redis                                                latest              a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask                           flask               788ca94b2313        8 months ago        694 MB
```

## <a name="push-images-to-registry"></a><span data-ttu-id="a0a4a-144">Push bitové kopie do registru.</span><span class="sxs-lookup"><span data-stu-id="a0a4a-144">Push images to registry</span></span>

<span data-ttu-id="a0a4a-145">Push *azure hlas front* bitovou kopii do registru.</span><span class="sxs-lookup"><span data-stu-id="a0a4a-145">Push the *azure-vote-front* image to the registry.</span></span> 

<span data-ttu-id="a0a4a-146">Pomocí následujícího příkladu, nahraďte název ACR loginServer loginServer ze svého prostředí.</span><span class="sxs-lookup"><span data-stu-id="a0a4a-146">Using the following example, replace the ACR loginServer name with the loginServer from your environment.</span></span>

```bash
docker push <acrLoginServer>/azure-vote-front:redis-v1
```

<span data-ttu-id="a0a4a-147">Tato akce trvá několik minut na dokončení.</span><span class="sxs-lookup"><span data-stu-id="a0a4a-147">This takes a couple of minutes to complete.</span></span>

## <a name="list-images-in-registry"></a><span data-ttu-id="a0a4a-148">Seznam obrázků v registru</span><span class="sxs-lookup"><span data-stu-id="a0a4a-148">List images in registry</span></span>

<span data-ttu-id="a0a4a-149">K zobrazení seznamu bitové kopie, které se nabídne do vašeho kontejneru Azure registru uživatele [az acr úložiště seznamu](/cli/azure/acr/repository#list) příkaz.</span><span class="sxs-lookup"><span data-stu-id="a0a4a-149">To return a list of images that have been pushed to your Azure Container registry, user the [az acr repository list](/cli/azure/acr/repository#list) command.</span></span> <span data-ttu-id="a0a4a-150">Aktualizujte příkaz s názvem instance ACR.</span><span class="sxs-lookup"><span data-stu-id="a0a4a-150">Update the command with the ACR instance name.</span></span>

```azurecli
az acr repository list --name <acrName> --output table
```

<span data-ttu-id="a0a4a-151">Výstup:</span><span class="sxs-lookup"><span data-stu-id="a0a4a-151">Output:</span></span>

```azurecli
Result
----------------
azure-vote-front
```

<span data-ttu-id="a0a4a-152">A pak najdete v části značky pro konkrétní image, pomocí [az acr úložiště zobrazit značky](/cli/azure/acr/repository#show-tags) příkaz.</span><span class="sxs-lookup"><span data-stu-id="a0a4a-152">And then to see the tags for a specific image, use the [az acr repository show-tags](/cli/azure/acr/repository#show-tags) command.</span></span>

```azurecli
az acr repository show-tags --name <acrName> --repository azure-vote-front --output table
```

<span data-ttu-id="a0a4a-153">Výstup:</span><span class="sxs-lookup"><span data-stu-id="a0a4a-153">Output:</span></span>

```azurecli
Result
--------
redis-v1
```

<span data-ttu-id="a0a4a-154">V kurzu dokončení bitovou kopii kontejneru byla uložena v privátní instanci Azure Container registru.</span><span class="sxs-lookup"><span data-stu-id="a0a4a-154">At tutorial completion, the container image has been stored in a private Azure Container Registry instance.</span></span> <span data-ttu-id="a0a4a-155">V následujících kurzech je nasadit tuto bitovou kopii z ACR do clusteru s podporou Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="a0a4a-155">This image is deployed from ACR to a Kubernetes cluster in subsequent tutorials.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a0a4a-156">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a0a4a-156">Next steps</span></span>

<span data-ttu-id="a0a4a-157">V tomto kurzu registru kontejner Azure připravené pro použití v clusteru služby ACS Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="a0a4a-157">In this tutorial, an Azure Container Registry was prepared for use in an ACS Kubernetes cluster.</span></span> <span data-ttu-id="a0a4a-158">Dokončili jste následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a0a4a-158">The following steps were completed:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a0a4a-159">Nasazení instanci Azure Container registru</span><span class="sxs-lookup"><span data-stu-id="a0a4a-159">Deployed an Azure Container Registry instance</span></span>
> * <span data-ttu-id="a0a4a-160">Označí image kontejner pro ACR</span><span class="sxs-lookup"><span data-stu-id="a0a4a-160">Tagged a container image for ACR</span></span>
> * <span data-ttu-id="a0a4a-161">Odeslat bitovou kopii do ACR</span><span class="sxs-lookup"><span data-stu-id="a0a4a-161">Uploaded the image to ACR</span></span>

<span data-ttu-id="a0a4a-162">Přechodu na v dalším kurzu se dozvíte o nasazení clusteru s podporou Kubernetes v Azure.</span><span class="sxs-lookup"><span data-stu-id="a0a4a-162">Advance to the next tutorial to learn about deploying a Kubernetes cluster in Azure.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="a0a4a-163">Nasazení clusteru Kubernetes</span><span class="sxs-lookup"><span data-stu-id="a0a4a-163">Deploy Kubernetes cluster</span></span>](./container-service-tutorial-kubernetes-deploy-cluster.md)