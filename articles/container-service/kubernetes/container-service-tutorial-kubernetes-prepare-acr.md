---
title: "kurz pro službu kontejneru aaaAzure – Příprava ACR | Microsoft Docs"
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
ms.openlocfilehash: 3980e5ce4eb9836f83c761a2f76c944bb3f13060
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-use-azure-container-registry"></a><span data-ttu-id="7e560-104">Nasazení a používání Azure kontejneru registru</span><span class="sxs-lookup"><span data-stu-id="7e560-104">Deploy and use Azure Container Registry</span></span>

<span data-ttu-id="7e560-105">Azure kontejneru registru (ACR) je založené na Azure, privátní registru, pro kontejner Docker bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="7e560-105">Azure Container Registry (ACR) is an Azure-based, private registry, for Docker container images.</span></span> <span data-ttu-id="7e560-106">Tento kurz, část 7, obě provede nasazení instanci Azure Container registru a předání tooit bitové kopie kontejneru.</span><span class="sxs-lookup"><span data-stu-id="7e560-106">This tutorial, part two of seven, walks through deploying an Azure Container Registry instance, and pushing a container image tooit.</span></span> <span data-ttu-id="7e560-107">Dokončit krokům patří:</span><span class="sxs-lookup"><span data-stu-id="7e560-107">Steps completed include:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="7e560-108">Nasazení instanci Azure Container registru (ACR)</span><span class="sxs-lookup"><span data-stu-id="7e560-108">Deploying an Azure Container Registry (ACR) instance</span></span>
> * <span data-ttu-id="7e560-109">Označování bitovou kopii kontejner pro ACR</span><span class="sxs-lookup"><span data-stu-id="7e560-109">Tagging a container image for ACR</span></span>
> * <span data-ttu-id="7e560-110">Odesílání bitové kopie tooACR hello</span><span class="sxs-lookup"><span data-stu-id="7e560-110">Uploading hello image tooACR</span></span>

<span data-ttu-id="7e560-111">V následujících kurzech této instance ACR je integrovaná do clusteru služby Azure Container Service Kubernetes pro zabezpečené spouštění kontejneru bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="7e560-111">In subsequent tutorials, this ACR instance is integrated with an Azure Container Service Kubernetes cluster, for securely running container images.</span></span> 

## <a name="before-you-begin"></a><span data-ttu-id="7e560-112">Než začnete</span><span class="sxs-lookup"><span data-stu-id="7e560-112">Before you begin</span></span>

<span data-ttu-id="7e560-113">V hello [předchozí kurzu](./container-service-tutorial-kubernetes-prepare-app.md), bitovou kopii kontejner byl vytvořen pro jednoduchou aplikaci Azure hlasování.</span><span class="sxs-lookup"><span data-stu-id="7e560-113">In hello [previous tutorial](./container-service-tutorial-kubernetes-prepare-app.md), a container image was created for a simple Azure Voting application.</span></span> <span data-ttu-id="7e560-114">V tomto kurzu se posune tuto bitovou kopii tooan registru kontejner Azure.</span><span class="sxs-lookup"><span data-stu-id="7e560-114">In this tutorial, this image is pushed tooan Azure Container Registry.</span></span> <span data-ttu-id="7e560-115">Pokud jste dosud nevytvořili hello obrázek aplikace Azure hlasování, vrátí příliš[kurzu 1 – Vytvoření kontejneru image](./container-service-tutorial-kubernetes-prepare-app.md).</span><span class="sxs-lookup"><span data-stu-id="7e560-115">If you have not created hello Azure Voting app image, return too[Tutorial 1 – Create container images](./container-service-tutorial-kubernetes-prepare-app.md).</span></span> <span data-ttu-id="7e560-116">Alternativně hello kroky podrobné zde pracují se žádný obrázek kontejneru.</span><span class="sxs-lookup"><span data-stu-id="7e560-116">Alternatively, hello steps detailed here work with any container image.</span></span>

<span data-ttu-id="7e560-117">Tento kurz vyžaduje, že používáte verzi rozhraní příkazového řádku Azure hello verze 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="7e560-117">This tutorial requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="7e560-118">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="7e560-118">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="7e560-119">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="7e560-119">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="deploy-azure-container-registry"></a><span data-ttu-id="7e560-120">Nasadit kontejner Azure registru</span><span class="sxs-lookup"><span data-stu-id="7e560-120">Deploy Azure Container Registry</span></span>

<span data-ttu-id="7e560-121">Pokud nasazujete registru kontejneru služby Azure, musíte nejprve skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="7e560-121">When deploying an Azure Container Registry, you first need a resource group.</span></span> <span data-ttu-id="7e560-122">Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="7e560-122">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span>

<span data-ttu-id="7e560-123">Vytvořte skupinu prostředků s hello [vytvořit skupinu az](/cli/azure/group#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="7e560-123">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="7e560-124">V tomto příkladu skupinu prostředků s názvem *myResourceGroup* je vytvořen v hello *westeurope* oblast.</span><span class="sxs-lookup"><span data-stu-id="7e560-124">In this example, a resource group named *myResourceGroup* is created in hello *westeurope* region.</span></span>

```azurecli
az group create --name myResourceGroup --location westeurope
```

<span data-ttu-id="7e560-125">Vytvoření registru kontejneru Azure s hello [vytvořit az acr](/cli/azure/acr#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="7e560-125">Create an Azure Container registry with hello [az acr create](/cli/azure/acr#create) command.</span></span> <span data-ttu-id="7e560-126">název kontejneru registru Hello **musí být jedinečné**.</span><span class="sxs-lookup"><span data-stu-id="7e560-126">hello name of a Container Registry **must be unique**.</span></span>

```azurecli
az acr create --resource-group myResourceGroup --name <acrName> --sku Basic --admin-enabled true
```

<span data-ttu-id="7e560-127">V rámci hello zbytek tohoto kurzu používáme "acrname" jako zástupný symbol pro název registru hello kontejneru, který jste si zvolili.</span><span class="sxs-lookup"><span data-stu-id="7e560-127">Throughout hello rest of this tutorial, we use "acrname" as a placeholder for hello container registry name that you chose.</span></span>

## <a name="container-registry-login"></a><span data-ttu-id="7e560-128">Kontejner registru přihlášení</span><span class="sxs-lookup"><span data-stu-id="7e560-128">Container registry login</span></span>

<span data-ttu-id="7e560-129">Musíte se přihlásit v instanci ACR tooyour před odesláním tooit bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="7e560-129">You must log in tooyour ACR instance before pushing images tooit.</span></span> <span data-ttu-id="7e560-130">Použití hello [az acr přihlášení](https://docs.microsoft.com/en-us/cli/azure/acr#login) příkaz toocomplete hello operaci.</span><span class="sxs-lookup"><span data-stu-id="7e560-130">Use hello [az acr login](https://docs.microsoft.com/en-us/cli/azure/acr#login) command toocomplete hello operation.</span></span> <span data-ttu-id="7e560-131">Je nutné tooprovide hello jedinečný název zadané toohello kontejneru registru, v okamžiku vytvoření.</span><span class="sxs-lookup"><span data-stu-id="7e560-131">You need tooprovide hello unique name given toohello container registry when it was created.</span></span>

```azurecli
az acr login --name <acrName>
```

<span data-ttu-id="7e560-132">příkaz Hello vrátí zprávu byla úspěšná přihlášení po dokončení.</span><span class="sxs-lookup"><span data-stu-id="7e560-132">hello command returns a 'Login Succeeded’ message once completed.</span></span>

## <a name="tag-container-images"></a><span data-ttu-id="7e560-133">Značka kontejneru obrázků</span><span class="sxs-lookup"><span data-stu-id="7e560-133">Tag container images</span></span>

<span data-ttu-id="7e560-134">Každý kontejner image musí toobe označené hello loginServer název registru hello.</span><span class="sxs-lookup"><span data-stu-id="7e560-134">Each container image needs toobe tagged with hello loginServer name of hello registry.</span></span> <span data-ttu-id="7e560-135">Tato značka se používá pro směrování při nabízení kontejneru bitové kopie tooan image registru.</span><span class="sxs-lookup"><span data-stu-id="7e560-135">This tag is used for routing when pushing container images tooan image registry.</span></span>

<span data-ttu-id="7e560-136">toosee seznam aktuální Image, použijte hello [imagí dockeru](https://docs.docker.com/engine/reference/commandline/images/) příkaz.</span><span class="sxs-lookup"><span data-stu-id="7e560-136">toosee a list of current images, use hello [docker images](https://docs.docker.com/engine/reference/commandline/images/) command.</span></span>

```bash
docker images
```

<span data-ttu-id="7e560-137">Výstup:</span><span class="sxs-lookup"><span data-stu-id="7e560-137">Output:</span></span>

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED             SIZE
azure-vote-front             latest              4675398c9172        13 minutes ago      694MB
redis                        latest              a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask   flask               788ca94b2313        9 months ago        694MB
```

<span data-ttu-id="7e560-138">tooget hello loginServer název, spusťte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="7e560-138">tooget hello loginServer name, run hello following command.</span></span>

```azurecli
az acr show --name <acrName> --query loginServer --output table
```

<span data-ttu-id="7e560-139">Nyní, značka hello *azure hlas front* bitovou kopii s loginServer hello hello kontejneru registru.</span><span class="sxs-lookup"><span data-stu-id="7e560-139">Now, tag hello *azure-vote-front* image with hello loginServer of hello container registry.</span></span> <span data-ttu-id="7e560-140">Navíc přidat `:redis-v1` toohello konec hello název bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="7e560-140">Also, add `:redis-v1` toohello end of hello image name.</span></span> <span data-ttu-id="7e560-141">Tato značka označuje verzi obrázku hello.</span><span class="sxs-lookup"><span data-stu-id="7e560-141">This tag indicates hello image version.</span></span>

```bash
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:redis-v1
```

<span data-ttu-id="7e560-142">Označí, spusťte po [imagí dockeru] (https://docs.docker.com/engine/reference/commandline/images/) tooverify hello operaci.</span><span class="sxs-lookup"><span data-stu-id="7e560-142">Once tagged, run [docker images] (https://docs.docker.com/engine/reference/commandline/images/) tooverify hello operation.</span></span>

```bash
docker images
```

<span data-ttu-id="7e560-143">Výstup:</span><span class="sxs-lookup"><span data-stu-id="7e560-143">Output:</span></span>

```bash
REPOSITORY                                           TAG                 IMAGE ID            CREATED             SIZE
azure-vote-front                                     latest              eaf2b9c57e5e        8 minutes ago       716 MB
mycontainerregistry082.azurecr.io/azure-vote-front   redis-v1            eaf2b9c57e5e        8 minutes ago       716 MB
redis                                                latest              a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask                           flask               788ca94b2313        8 months ago        694 MB
```

## <a name="push-images-tooregistry"></a><span data-ttu-id="7e560-144">Push tooregistry bitové kopie</span><span class="sxs-lookup"><span data-stu-id="7e560-144">Push images tooregistry</span></span>

<span data-ttu-id="7e560-145">Push hello *azure hlas front* registru toohello bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="7e560-145">Push hello *azure-vote-front* image toohello registry.</span></span> 

<span data-ttu-id="7e560-146">Pomocí následující ukázka hello, nahraďte název loginServer ACR hello hello loginServer ze svého prostředí.</span><span class="sxs-lookup"><span data-stu-id="7e560-146">Using hello following example, replace hello ACR loginServer name with hello loginServer from your environment.</span></span>

```bash
docker push <acrLoginServer>/azure-vote-front:redis-v1
```

<span data-ttu-id="7e560-147">To bude trvat několik minut toocomplete.</span><span class="sxs-lookup"><span data-stu-id="7e560-147">This takes a couple of minutes toocomplete.</span></span>

## <a name="list-images-in-registry"></a><span data-ttu-id="7e560-148">Seznam obrázků v registru</span><span class="sxs-lookup"><span data-stu-id="7e560-148">List images in registry</span></span>

<span data-ttu-id="7e560-149">tooreturn seznam bitové kopie, které se nabídne tooyour kontejneru Azure registru, uživatel hello [az acr úložiště seznamu](/cli/azure/acr/repository#list) příkaz.</span><span class="sxs-lookup"><span data-stu-id="7e560-149">tooreturn a list of images that have been pushed tooyour Azure Container registry, user hello [az acr repository list](/cli/azure/acr/repository#list) command.</span></span> <span data-ttu-id="7e560-150">Aktualizujte hello příkaz s názvem instance ACR hello.</span><span class="sxs-lookup"><span data-stu-id="7e560-150">Update hello command with hello ACR instance name.</span></span>

```azurecli
az acr repository list --name <acrName> --output table
```

<span data-ttu-id="7e560-151">Výstup:</span><span class="sxs-lookup"><span data-stu-id="7e560-151">Output:</span></span>

```azurecli
Result
----------------
azure-vote-front
```

<span data-ttu-id="7e560-152">A pak toosee hello značky pro konkrétní image, pomocí hello [az acr úložiště zobrazit značky](/cli/azure/acr/repository#show-tags) příkaz.</span><span class="sxs-lookup"><span data-stu-id="7e560-152">And then toosee hello tags for a specific image, use hello [az acr repository show-tags](/cli/azure/acr/repository#show-tags) command.</span></span>

```azurecli
az acr repository show-tags --name <acrName> --repository azure-vote-front --output table
```

<span data-ttu-id="7e560-153">Výstup:</span><span class="sxs-lookup"><span data-stu-id="7e560-153">Output:</span></span>

```azurecli
Result
--------
redis-v1
```

<span data-ttu-id="7e560-154">V kurzu dokončení hello kontejneru image byla uložena v privátní instanci Azure Container registru.</span><span class="sxs-lookup"><span data-stu-id="7e560-154">At tutorial completion, hello container image has been stored in a private Azure Container Registry instance.</span></span> <span data-ttu-id="7e560-155">Tato bitová kopie nasazována z ACR tooa Kubernetes clusteru v následujících kurzech.</span><span class="sxs-lookup"><span data-stu-id="7e560-155">This image is deployed from ACR tooa Kubernetes cluster in subsequent tutorials.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7e560-156">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7e560-156">Next steps</span></span>

<span data-ttu-id="7e560-157">V tomto kurzu registru kontejner Azure připravené pro použití v clusteru služby ACS Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="7e560-157">In this tutorial, an Azure Container Registry was prepared for use in an ACS Kubernetes cluster.</span></span> <span data-ttu-id="7e560-158">byly dokončeny Hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="7e560-158">hello following steps were completed:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="7e560-159">Nasazení instanci Azure Container registru</span><span class="sxs-lookup"><span data-stu-id="7e560-159">Deployed an Azure Container Registry instance</span></span>
> * <span data-ttu-id="7e560-160">Označí image kontejner pro ACR</span><span class="sxs-lookup"><span data-stu-id="7e560-160">Tagged a container image for ACR</span></span>
> * <span data-ttu-id="7e560-161">TooACR image nahrané hello</span><span class="sxs-lookup"><span data-stu-id="7e560-161">Uploaded hello image tooACR</span></span>

<span data-ttu-id="7e560-162">Posunutí další kurz toolearn toohello o nasazení clusteru s podporou Kubernetes v Azure.</span><span class="sxs-lookup"><span data-stu-id="7e560-162">Advance toohello next tutorial toolearn about deploying a Kubernetes cluster in Azure.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="7e560-163">Nasazení clusteru Kubernetes</span><span class="sxs-lookup"><span data-stu-id="7e560-163">Deploy Kubernetes cluster</span></span>](./container-service-tutorial-kubernetes-deploy-cluster.md)