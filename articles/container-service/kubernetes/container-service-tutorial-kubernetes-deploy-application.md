---
title: "Kurz pro Azure Container Service – nasazení aplikace | Microsoft Docs"
description: "Kurz pro Azure Container Service – nasazení aplikace"
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, Kontejnery, mikroslužby, Kubernetes, DC/OS, Azure"
ms.assetid: 
ms.service: container-service
ms.devlang: aurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: ea67f0beb6a5926393b26e7590302ad0f46a63f9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="run-applications-in-kubernetes"></a><span data-ttu-id="2fa21-104">Spuštění aplikace v Kubernetes</span><span class="sxs-lookup"><span data-stu-id="2fa21-104">Run applications in Kubernetes</span></span>

<span data-ttu-id="2fa21-105">V tomto kurzu součástí čtyři 7, vzorová aplikace je nasazený do clusteru s podporou Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="2fa21-105">In this tutorial, part four of seven, a sample application is deployed into a Kubernetes cluster.</span></span> <span data-ttu-id="2fa21-106">Dokončit krokům patří:</span><span class="sxs-lookup"><span data-stu-id="2fa21-106">Steps completed include:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2fa21-107">Stáhnout soubory manifestu Kubernetes</span><span class="sxs-lookup"><span data-stu-id="2fa21-107">Download Kubernetes manifest files</span></span>
> * <span data-ttu-id="2fa21-108">Spuštění aplikace v Kubernetes</span><span class="sxs-lookup"><span data-stu-id="2fa21-108">Run application in Kubernetes</span></span>
> * <span data-ttu-id="2fa21-109">Testování aplikace</span><span class="sxs-lookup"><span data-stu-id="2fa21-109">Test the application</span></span>

<span data-ttu-id="2fa21-110">V následujících kurzech této aplikace je škálovat na více systémů, aktualizovat, a Operations Management Suite konfigurované pro monitorování Kubernetes clusteru.</span><span class="sxs-lookup"><span data-stu-id="2fa21-110">In subsequent tutorials, this application is scaled out, updated, and Operations Management Suite configured to monitor the Kubernetes cluster.</span></span>

<span data-ttu-id="2fa21-111">Tento kurz předpokládá základní znalosti o Kubernetes koncepty, podrobné informace o Kubernetes najdete [Kubernetes dokumentaci](https://kubernetes.io/docs/home/).</span><span class="sxs-lookup"><span data-stu-id="2fa21-111">This tutorial assumes a basic understanding of Kubernetes concepts, for detailed information on Kubernetes see the [Kubernetes documentation](https://kubernetes.io/docs/home/).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="2fa21-112">Než začnete</span><span class="sxs-lookup"><span data-stu-id="2fa21-112">Before you begin</span></span>

<span data-ttu-id="2fa21-113">V předchozí kurzy aplikace byla zabalené do kontejneru image, tuto bitovou kopii byl odeslán do registru kontejner Azure a Kubernetes cluster byla vytvořena.</span><span class="sxs-lookup"><span data-stu-id="2fa21-113">In previous tutorials, an application was packaged into a container image, this image was uploaded to Azure Container Registry, and a Kubernetes cluster was created.</span></span> <span data-ttu-id="2fa21-114">Pokud se ještě provést tyto kroky a chcete sledovat, vrátit [kurzu 1 – Vytvoření kontejneru image](./container-service-tutorial-kubernetes-prepare-app.md).</span><span class="sxs-lookup"><span data-stu-id="2fa21-114">If you have not done these steps, and would like to follow along, return to [Tutorial 1 – Create container images](./container-service-tutorial-kubernetes-prepare-app.md).</span></span> 

<span data-ttu-id="2fa21-115">Minimálně tento kurz vyžaduje Kubernetes clusteru.</span><span class="sxs-lookup"><span data-stu-id="2fa21-115">At minimum, this tutorial requires a Kubernetes cluster.</span></span>

## <a name="get-manifest-file"></a><span data-ttu-id="2fa21-116">Získat soubor manifestu</span><span class="sxs-lookup"><span data-stu-id="2fa21-116">Get manifest file</span></span>

<span data-ttu-id="2fa21-117">V tomto kurzu [Kubernetes objekty](https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/) jsou nasazeny pomocí Kubernetes manifestu.</span><span class="sxs-lookup"><span data-stu-id="2fa21-117">For this tutorial, [Kubernetes objects](https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/) are deployed using a Kubernetes manifest.</span></span> <span data-ttu-id="2fa21-118">Kubernetes manifest je soubor YAML nebo JSON formátovaný obsahující pokyny k nasazení a konfigurace objektu Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="2fa21-118">A Kubernetes manifest is a YAML or JSON formatted file containing Kubernetes object deployment and configuration instructions.</span></span>

<span data-ttu-id="2fa21-119">Soubor manifestu aplikace pro účely tohoto kurzu je k dispozici v úložišti aplikací Azure hlas, který byl v předchozím kurzu klonovat.</span><span class="sxs-lookup"><span data-stu-id="2fa21-119">The application manifest file for this tutorial is available in the Azure Vote application repo, which was cloned in a previous tutorial.</span></span> <span data-ttu-id="2fa21-120">Pokud jste tak již neučinili, naklonujte úložiště pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="2fa21-120">If you have not already done so, clone the repo with the following command:</span></span> 

```bash
git clone https://github.com/Azure-Samples/azure-voting-app-redis.git
```

<span data-ttu-id="2fa21-121">Soubor manifestu se nachází v následujícím adresáři klonovaný úložišti.</span><span class="sxs-lookup"><span data-stu-id="2fa21-121">The manifest file is found in the following directory of the cloned repo.</span></span>

```bash
/azure-voting-app-redis/kubernetes-manifests/azure-vote-all-in-one-redis.yml
```

## <a name="update-manifest-file"></a><span data-ttu-id="2fa21-122">Aktualizace souboru manifestu</span><span class="sxs-lookup"><span data-stu-id="2fa21-122">Update manifest file</span></span>

<span data-ttu-id="2fa21-123">Pokud používáte Azure kontejneru registru pro ukládání bitových kopií kontejneru, je třeba aktualizovat s názvem loginServer ACR manifest.</span><span class="sxs-lookup"><span data-stu-id="2fa21-123">If using Azure Container Registry to store the container images, the manifest needs to be updated with the ACR loginServer name.</span></span>

<span data-ttu-id="2fa21-124">Získat název ACR přihlášení serveru s [az acr seznamu](/cli/azure/acr#list) příkaz.</span><span class="sxs-lookup"><span data-stu-id="2fa21-124">Get the ACR login server name with the [az acr list](/cli/azure/acr#list) command.</span></span>

```azurecli-interactive
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

<span data-ttu-id="2fa21-125">Ukázka manifest byl předem vytvořené s názvem úložiště *microsoft*.</span><span class="sxs-lookup"><span data-stu-id="2fa21-125">The sample manifest has been pre-created with a repository name of *microsoft*.</span></span> <span data-ttu-id="2fa21-126">V každém textovém editoru otevřete soubor a nahraďte *microsoft* hodnotu s názvem serveru přihlášení vaší instance ACR.</span><span class="sxs-lookup"><span data-stu-id="2fa21-126">Open the file with any text editor, and replace the *microsoft* value with the login server name of your ACR instance.</span></span>

```yaml
containers:
- name: azure-vote-front
  image: microsoft/azure-vote-front:redis-v1
```

## <a name="deploy-application"></a><span data-ttu-id="2fa21-127">Nasazení aplikace</span><span class="sxs-lookup"><span data-stu-id="2fa21-127">Deploy application</span></span>

<span data-ttu-id="2fa21-128">Pomocí příkazu [kubectl create](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#create) spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2fa21-128">Use the [kubectl create](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#create) command to run the application.</span></span> <span data-ttu-id="2fa21-129">Tento příkaz analyzuje souboru manifestu a vytvořit objekty definované Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="2fa21-129">This command parses the manifest file and create the defined Kubernetes objects.</span></span>

```azurecli-interactive
kubectl create -f ./azure-voting-app-redis/kubernetes-manifests/azure-vote-all-in-one-redis.yml
```

<span data-ttu-id="2fa21-130">Výstup:</span><span class="sxs-lookup"><span data-stu-id="2fa21-130">Output:</span></span>

```bash
deployment "azure-vote-back" created
service "azure-vote-back" created
deployment "azure-vote-front" created
service "azure-vote-front" created
```

## <a name="test-application"></a><span data-ttu-id="2fa21-131">Testování aplikace</span><span class="sxs-lookup"><span data-stu-id="2fa21-131">Test application</span></span>

<span data-ttu-id="2fa21-132">A [Kubernetes služby](https://kubernetes.io/docs/concepts/services-networking/service/) se vytvoří, který zpřístupňuje aplikace k Internetu.</span><span class="sxs-lookup"><span data-stu-id="2fa21-132">A [Kubernetes service](https://kubernetes.io/docs/concepts/services-networking/service/) is created which exposes the application to the internet.</span></span> <span data-ttu-id="2fa21-133">Tento proces může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="2fa21-133">This process can take a few minutes.</span></span> 

<span data-ttu-id="2fa21-134">Pomocí příkazu [kubectl get service](https://review.docs.microsoft.com/en-us/azure/container-service/container-service-kubernetes-walkthrough?branch=pr-en-us-17681) s argumentem `--watch` můžete sledovat průběh.</span><span class="sxs-lookup"><span data-stu-id="2fa21-134">To monitor progress, use the [kubectl get service](https://review.docs.microsoft.com/en-us/azure/container-service/container-service-kubernetes-walkthrough?branch=pr-en-us-17681) command with the `--watch` argument.</span></span>

```azurecli-interactive
kubectl get service azure-vote-front --watch
```

<span data-ttu-id="2fa21-135">Zpočátku **externí IP** pro *azure hlas front* služby se zobrazí jako *čekající*.</span><span class="sxs-lookup"><span data-stu-id="2fa21-135">Initially, the **EXTERNAL-IP** for the *azure-vote-front* service appears as *pending*.</span></span> <span data-ttu-id="2fa21-136">Jakmile se adresa EXTERNAL-IP změní ze stavu *probíhá* na *IP adresu*, pomocí klávesové zkratky `CTRL-C` zastavte sledovací proces kubectl.</span><span class="sxs-lookup"><span data-stu-id="2fa21-136">Once the EXTERNAL-IP address has changed from *pending* to an *IP address*, use `CTRL-C` to stop the kubectl watch process.</span></span>

```bash
NAME               CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   10.0.42.158   <pending>     80:31873/TCP   1m
azure-vote-front   10.0.42.158   52.179.23.131 80:31873/TCP   2m
```

<span data-ttu-id="2fa21-137">Informace o aplikaci, přejděte na externí IP adresu.</span><span class="sxs-lookup"><span data-stu-id="2fa21-137">To see the application, browse to the external IP address.</span></span>

![Obrázek clusteru Kubernetes v Azure](media/container-service-kubernetes-tutorials/azure-vote.png)

## <a name="next-steps"></a><span data-ttu-id="2fa21-139">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2fa21-139">Next steps</span></span>

<span data-ttu-id="2fa21-140">V tomto kurzu aplikace Azure hlas nasazená do clusteru Azure Container Service Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="2fa21-140">In this tutorial, the Azure vote application was deployed to an Azure Container Service Kubernetes cluster.</span></span> <span data-ttu-id="2fa21-141">Dokončené úkoly patří:</span><span class="sxs-lookup"><span data-stu-id="2fa21-141">Tasks completed include:</span></span>  

> [!div class="checklist"]
> * <span data-ttu-id="2fa21-142">Stáhnout soubory manifestu Kubernetes</span><span class="sxs-lookup"><span data-stu-id="2fa21-142">Download Kubernetes manifest files</span></span>
> * <span data-ttu-id="2fa21-143">Spusťte aplikaci v Kubernetes</span><span class="sxs-lookup"><span data-stu-id="2fa21-143">Run the application in Kubernetes</span></span>
> * <span data-ttu-id="2fa21-144">Testování aplikace</span><span class="sxs-lookup"><span data-stu-id="2fa21-144">Tested the application</span></span>

<span data-ttu-id="2fa21-145">Přechodu na v dalším kurzu se dozvíte o škálování Kubernetes aplikace a podpůrné infrastruktuře Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="2fa21-145">Advance to the next tutorial to learn about scaling both a Kubernetes application and the underlying Kubernetes infrastructure.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="2fa21-146">Škálování Kubernetes aplikace a infrastrukturu</span><span class="sxs-lookup"><span data-stu-id="2fa21-146">Scale Kubernetes application and infrastructure</span></span>](./container-service-tutorial-kubernetes-scale.md)