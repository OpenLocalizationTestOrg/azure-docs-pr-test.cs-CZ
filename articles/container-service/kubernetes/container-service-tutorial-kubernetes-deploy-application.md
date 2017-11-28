---
title: "kurz pro službu kontejneru aaaAzure – nasazení aplikace | Microsoft Docs"
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
ms.openlocfilehash: 7e2fa06d359caf83e684df3966624a6e9a8e7efa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-applications-in-kubernetes"></a><span data-ttu-id="5a8f5-104">Spuštění aplikace v Kubernetes</span><span class="sxs-lookup"><span data-stu-id="5a8f5-104">Run applications in Kubernetes</span></span>

<span data-ttu-id="5a8f5-105">V tomto kurzu součástí čtyři 7, vzorová aplikace je nasazený do clusteru s podporou Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="5a8f5-105">In this tutorial, part four of seven, a sample application is deployed into a Kubernetes cluster.</span></span> <span data-ttu-id="5a8f5-106">Dokončit krokům patří:</span><span class="sxs-lookup"><span data-stu-id="5a8f5-106">Steps completed include:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="5a8f5-107">Stáhnout soubory manifestu Kubernetes</span><span class="sxs-lookup"><span data-stu-id="5a8f5-107">Download Kubernetes manifest files</span></span>
> * <span data-ttu-id="5a8f5-108">Spuštění aplikace v Kubernetes</span><span class="sxs-lookup"><span data-stu-id="5a8f5-108">Run application in Kubernetes</span></span>
> * <span data-ttu-id="5a8f5-109">Testování aplikace hello</span><span class="sxs-lookup"><span data-stu-id="5a8f5-109">Test hello application</span></span>

<span data-ttu-id="5a8f5-110">V následujících kurzech této aplikace je škálovat na více systémů, aktualizovat, a nakonfigurovat Operations Management Suite toomonitor hello Kubernetes clusteru.</span><span class="sxs-lookup"><span data-stu-id="5a8f5-110">In subsequent tutorials, this application is scaled out, updated, and Operations Management Suite configured toomonitor hello Kubernetes cluster.</span></span>

<span data-ttu-id="5a8f5-111">Tento kurz předpokládá základní znalosti koncepce Kubernetes najdete podrobné informace o Kubernetes hello [Kubernetes dokumentaci](https://kubernetes.io/docs/home/).</span><span class="sxs-lookup"><span data-stu-id="5a8f5-111">This tutorial assumes a basic understanding of Kubernetes concepts, for detailed information on Kubernetes see hello [Kubernetes documentation](https://kubernetes.io/docs/home/).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="5a8f5-112">Než začnete</span><span class="sxs-lookup"><span data-stu-id="5a8f5-112">Before you begin</span></span>

<span data-ttu-id="5a8f5-113">V předchozí kurzy aplikace se zabalí do kontejneru image, tuto bitovou kopii, byla nahrané tooAzure registru kontejneru a byl vytvořen Kubernetes cluster.</span><span class="sxs-lookup"><span data-stu-id="5a8f5-113">In previous tutorials, an application was packaged into a container image, this image was uploaded tooAzure Container Registry, and a Kubernetes cluster was created.</span></span> <span data-ttu-id="5a8f5-114">Pokud jste ještě provést tyto kroky a chcete toofollow společně, vrátí příliš[kurzu 1 – Vytvoření kontejneru image](./container-service-tutorial-kubernetes-prepare-app.md).</span><span class="sxs-lookup"><span data-stu-id="5a8f5-114">If you have not done these steps, and would like toofollow along, return too[Tutorial 1 – Create container images](./container-service-tutorial-kubernetes-prepare-app.md).</span></span> 

<span data-ttu-id="5a8f5-115">Minimálně tento kurz vyžaduje Kubernetes clusteru.</span><span class="sxs-lookup"><span data-stu-id="5a8f5-115">At minimum, this tutorial requires a Kubernetes cluster.</span></span>

## <a name="get-manifest-file"></a><span data-ttu-id="5a8f5-116">Získat soubor manifestu</span><span class="sxs-lookup"><span data-stu-id="5a8f5-116">Get manifest file</span></span>

<span data-ttu-id="5a8f5-117">V tomto kurzu [Kubernetes objekty](https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/) jsou nasazeny pomocí Kubernetes manifestu.</span><span class="sxs-lookup"><span data-stu-id="5a8f5-117">For this tutorial, [Kubernetes objects](https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/) are deployed using a Kubernetes manifest.</span></span> <span data-ttu-id="5a8f5-118">Kubernetes manifest je soubor YAML nebo JSON formátovaný obsahující pokyny k nasazení a konfigurace objektu Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="5a8f5-118">A Kubernetes manifest is a YAML or JSON formatted file containing Kubernetes object deployment and configuration instructions.</span></span>

<span data-ttu-id="5a8f5-119">Hello souboru manifestu aplikace pro účely tohoto kurzu je k dispozici v úložišti aplikací Azure hlas hello, který byl v předchozím kurzu klonovat.</span><span class="sxs-lookup"><span data-stu-id="5a8f5-119">hello application manifest file for this tutorial is available in hello Azure Vote application repo, which was cloned in a previous tutorial.</span></span> <span data-ttu-id="5a8f5-120">Pokud jste tak již neučinili, klonovat úložiště hello s hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="5a8f5-120">If you have not already done so, clone hello repo with hello following command:</span></span> 

```bash
git clone https://github.com/Azure-Samples/azure-voting-app-redis.git
```

<span data-ttu-id="5a8f5-121">Soubor manifestu Hello nachází v následujícím adresáři hello klonovat úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="5a8f5-121">hello manifest file is found in hello following directory of hello cloned repo.</span></span>

```bash
/azure-voting-app-redis/kubernetes-manifests/azure-vote-all-in-one-redis.yml
```

## <a name="update-manifest-file"></a><span data-ttu-id="5a8f5-122">Aktualizace souboru manifestu</span><span class="sxs-lookup"><span data-stu-id="5a8f5-122">Update manifest file</span></span>

<span data-ttu-id="5a8f5-123">Pokud používáte Azure kontejneru registru toostore hello kontejneru obrázky, hello manifestu potřebám toobe aktualizované hello loginServer název ACR.</span><span class="sxs-lookup"><span data-stu-id="5a8f5-123">If using Azure Container Registry toostore hello container images, hello manifest needs toobe updated with hello ACR loginServer name.</span></span>

<span data-ttu-id="5a8f5-124">Získat název serveru aplikace hello ACR přihlášení s hello [az acr seznamu](/cli/azure/acr#list) příkaz.</span><span class="sxs-lookup"><span data-stu-id="5a8f5-124">Get hello ACR login server name with hello [az acr list](/cli/azure/acr#list) command.</span></span>

```azurecli-interactive
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

<span data-ttu-id="5a8f5-125">Hello manifest ukázka byla předem vytvořené s názvem úložiště *microsoft*.</span><span class="sxs-lookup"><span data-stu-id="5a8f5-125">hello sample manifest has been pre-created with a repository name of *microsoft*.</span></span> <span data-ttu-id="5a8f5-126">Otevřete soubor hello v každém textovém editoru a nahraďte hello *microsoft* hodnotu s názvem serveru hello přihlášení vaší instance ACR.</span><span class="sxs-lookup"><span data-stu-id="5a8f5-126">Open hello file with any text editor, and replace hello *microsoft* value with hello login server name of your ACR instance.</span></span>

```yaml
containers:
- name: azure-vote-front
  image: microsoft/azure-vote-front:redis-v1
```

## <a name="deploy-application"></a><span data-ttu-id="5a8f5-127">Nasazení aplikace</span><span class="sxs-lookup"><span data-stu-id="5a8f5-127">Deploy application</span></span>

<span data-ttu-id="5a8f5-128">Použití hello [kubectl vytvořit](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#create) příkaz toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="5a8f5-128">Use hello [kubectl create](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#create) command toorun hello application.</span></span> <span data-ttu-id="5a8f5-129">Tento příkaz analyzuje hello soubor manifestu a vytvořit objekty Kubernetes hello definované.</span><span class="sxs-lookup"><span data-stu-id="5a8f5-129">This command parses hello manifest file and create hello defined Kubernetes objects.</span></span>

```azurecli-interactive
kubectl create -f ./azure-voting-app-redis/kubernetes-manifests/azure-vote-all-in-one-redis.yml
```

<span data-ttu-id="5a8f5-130">Výstup:</span><span class="sxs-lookup"><span data-stu-id="5a8f5-130">Output:</span></span>

```bash
deployment "azure-vote-back" created
service "azure-vote-back" created
deployment "azure-vote-front" created
service "azure-vote-front" created
```

## <a name="test-application"></a><span data-ttu-id="5a8f5-131">Testování aplikace</span><span class="sxs-lookup"><span data-stu-id="5a8f5-131">Test application</span></span>

<span data-ttu-id="5a8f5-132">A [Kubernetes služby](https://kubernetes.io/docs/concepts/services-networking/service/) se vytvoří, který zpřístupňuje toohello aplikace hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="5a8f5-132">A [Kubernetes service](https://kubernetes.io/docs/concepts/services-networking/service/) is created which exposes hello application toohello internet.</span></span> <span data-ttu-id="5a8f5-133">Tento proces může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="5a8f5-133">This process can take a few minutes.</span></span> 

<span data-ttu-id="5a8f5-134">toomonitor průběh, použijte hello [kubectl získat služby](https://review.docs.microsoft.com/en-us/azure/container-service/container-service-kubernetes-walkthrough?branch=pr-en-us-17681) s hello `--watch` argument.</span><span class="sxs-lookup"><span data-stu-id="5a8f5-134">toomonitor progress, use hello [kubectl get service](https://review.docs.microsoft.com/en-us/azure/container-service/container-service-kubernetes-walkthrough?branch=pr-en-us-17681) command with hello `--watch` argument.</span></span>

```azurecli-interactive
kubectl get service azure-vote-front --watch
```

<span data-ttu-id="5a8f5-135">Na začátku hello **externí IP** pro hello *azure hlas front* služby se zobrazí jako *čekající*.</span><span class="sxs-lookup"><span data-stu-id="5a8f5-135">Initially, hello **EXTERNAL-IP** for hello *azure-vote-front* service appears as *pending*.</span></span> <span data-ttu-id="5a8f5-136">Jakmile hello externí IP adresu se změnil z *čekající* tooan *IP adresu*, použijte `CTRL-C` toostop hello kubectl sledovat proces.</span><span class="sxs-lookup"><span data-stu-id="5a8f5-136">Once hello EXTERNAL-IP address has changed from *pending* tooan *IP address*, use `CTRL-C` toostop hello kubectl watch process.</span></span>

```bash
NAME               CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   10.0.42.158   <pending>     80:31873/TCP   1m
azure-vote-front   10.0.42.158   52.179.23.131 80:31873/TCP   2m
```

<span data-ttu-id="5a8f5-137">toosee hello aplikaci, procházejte toohello externí IP adresu.</span><span class="sxs-lookup"><span data-stu-id="5a8f5-137">toosee hello application, browse toohello external IP address.</span></span>

![Obrázek clusteru Kubernetes v Azure](media/container-service-kubernetes-tutorials/azure-vote.png)

## <a name="next-steps"></a><span data-ttu-id="5a8f5-139">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5a8f5-139">Next steps</span></span>

<span data-ttu-id="5a8f5-140">V tomto kurzu se hello Azure hlas aplikace nasazené tooan clusteru Azure Container Service Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="5a8f5-140">In this tutorial, hello Azure vote application was deployed tooan Azure Container Service Kubernetes cluster.</span></span> <span data-ttu-id="5a8f5-141">Dokončené úkoly patří:</span><span class="sxs-lookup"><span data-stu-id="5a8f5-141">Tasks completed include:</span></span>  

> [!div class="checklist"]
> * <span data-ttu-id="5a8f5-142">Stáhnout soubory manifestu Kubernetes</span><span class="sxs-lookup"><span data-stu-id="5a8f5-142">Download Kubernetes manifest files</span></span>
> * <span data-ttu-id="5a8f5-143">Spuštění aplikace hello v Kubernetes</span><span class="sxs-lookup"><span data-stu-id="5a8f5-143">Run hello application in Kubernetes</span></span>
> * <span data-ttu-id="5a8f5-144">Aplikace otestované hello</span><span class="sxs-lookup"><span data-stu-id="5a8f5-144">Tested hello application</span></span>

<span data-ttu-id="5a8f5-145">Posunutí další kurz toolearn toohello o škálování aplikace Kubernetes i hello základní Kubernetes infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="5a8f5-145">Advance toohello next tutorial toolearn about scaling both a Kubernetes application and hello underlying Kubernetes infrastructure.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="5a8f5-146">Škálování Kubernetes aplikace a infrastrukturu</span><span class="sxs-lookup"><span data-stu-id="5a8f5-146">Scale Kubernetes application and infrastructure</span></span>](./container-service-tutorial-kubernetes-scale.md)