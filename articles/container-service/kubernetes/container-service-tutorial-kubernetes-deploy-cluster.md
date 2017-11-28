---
title: "kurz pro službu kontejneru aaaAzure – nasazení clusteru | Microsoft Docs"
description: "Kurz pro Azure Container Service – nasazení clusteru"
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
ms.openlocfilehash: c4c8cc95c88d9c2077d0322f57e5d3159e2dd0ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-kubernetes-cluster-in-azure-container-service"></a><span data-ttu-id="f5d23-104">Nasazení clusteru Kubernetes v Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="f5d23-104">Deploy a Kubernetes cluster in Azure Container Service</span></span>

<span data-ttu-id="f5d23-105">Kubernetes poskytuje distribuovanou platformu pro kontejnerizované aplikace.</span><span class="sxs-lookup"><span data-stu-id="f5d23-105">Kubernetes provides a distributed platform for containerized applications.</span></span> <span data-ttu-id="f5d23-106">Zřizování clusteru výroby připravené Kubernetes s Azure Container Service je usnadňují a urychlují.</span><span class="sxs-lookup"><span data-stu-id="f5d23-106">With Azure Container Service, provisioning of a production ready Kubernetes cluster is simple and quick.</span></span> <span data-ttu-id="f5d23-107">V tomto kurzu, část 3 7, je nasazení clusteru Azure Container Service Kubernetes služby.</span><span class="sxs-lookup"><span data-stu-id="f5d23-107">In this tutorial, part 3 of 7, an Azure Container Service Kubernetes cluster is deployed.</span></span> <span data-ttu-id="f5d23-108">Dokončit krokům patří:</span><span class="sxs-lookup"><span data-stu-id="f5d23-108">Steps completed include:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f5d23-109">Nasazení služby ACS Kubernetes clusteru</span><span class="sxs-lookup"><span data-stu-id="f5d23-109">Deploying a Kubernetes ACS cluster</span></span>
> * <span data-ttu-id="f5d23-110">Instalace hello Kubernetes rozhraní příkazového řádku (kubectl)</span><span class="sxs-lookup"><span data-stu-id="f5d23-110">Installation of hello Kubernetes CLI (kubectl)</span></span>
> * <span data-ttu-id="f5d23-111">Konfigurace kubectl</span><span class="sxs-lookup"><span data-stu-id="f5d23-111">Configuration of kubectl</span></span>

<span data-ttu-id="f5d23-112">V následujících kurzech hello hlas Azure je aplikace nasazena toohello clusteru, škálovat, aktualizovat a Operations Management Suite je nakonfigurované toomonitor hello Kubernetes clusteru.</span><span class="sxs-lookup"><span data-stu-id="f5d23-112">In subsequent tutorials, hello Azure Vote application is deployed toohello cluster, scaled, updated, and Operations Management Suite is configured toomonitor hello Kubernetes cluster.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="f5d23-113">Než začnete</span><span class="sxs-lookup"><span data-stu-id="f5d23-113">Before you begin</span></span>

<span data-ttu-id="f5d23-114">V předchozích kurzy bitovou kopii kontejner byl vytvořen a nahrát tooan registru kontejner Azure instance.</span><span class="sxs-lookup"><span data-stu-id="f5d23-114">In previous tutorials, a container image was created and uploaded tooan Azure Container Registry instance.</span></span> <span data-ttu-id="f5d23-115">Pokud jste ještě provést tyto kroky a chcete toofollow společně, vrátí příliš[kurzu 1 – Vytvoření kontejneru image](./container-service-tutorial-kubernetes-prepare-app.md).</span><span class="sxs-lookup"><span data-stu-id="f5d23-115">If you have not done these steps, and would like toofollow along, return too[Tutorial 1 – Create container images](./container-service-tutorial-kubernetes-prepare-app.md).</span></span>

## <a name="create-kubernetes-cluster"></a><span data-ttu-id="f5d23-116">Vytvoření clusteru Kubernetes</span><span class="sxs-lookup"><span data-stu-id="f5d23-116">Create Kubernetes cluster</span></span>

<span data-ttu-id="f5d23-117">V hello [předchozí kurzu](./container-service-tutorial-kubernetes-prepare-acr.md), skupinu prostředků s názvem *myResourceGroup* byla vytvořena.</span><span class="sxs-lookup"><span data-stu-id="f5d23-117">In hello [previous tutorial](./container-service-tutorial-kubernetes-prepare-acr.md), a resource group named *myResourceGroup* was created.</span></span> <span data-ttu-id="f5d23-118">Pokud jste tak dosud neučinili, vytvořte tato skupina prostředků teď.</span><span class="sxs-lookup"><span data-stu-id="f5d23-118">If you have not done so, create this resource group now.</span></span>

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```

<span data-ttu-id="f5d23-119">Vytvoření clusteru Kubernetes v Azure Container Service s hello [vytvořit acs az](/cli/azure/acs#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="f5d23-119">Create a Kubernetes cluster in Azure Container Service with hello [az acs create](/cli/azure/acs#create) command.</span></span> 

<span data-ttu-id="f5d23-120">Hello následující příklad vytvoří cluster s názvem *myK8sCluster* s Linuxem jeden hlavní uzel a tři uzly Linux agent.</span><span class="sxs-lookup"><span data-stu-id="f5d23-120">hello following example creates a cluster named *myK8sCluster* with one Linux master node and three Linux agent nodes.</span></span>

```azurecli-interactive 
az acs create --orchestrator-type=kubernetes --resource-group myResourceGroup --name=myK8SCluster --generate-ssh-keys 
```

<span data-ttu-id="f5d23-121">Po několika minutách dokončení příkazu hello a formátu json vrátí informace o nasazení hello služby ACS.</span><span class="sxs-lookup"><span data-stu-id="f5d23-121">After several minutes, hello command completes, and returns json formatted information about hello ACS deployment.</span></span>

## <a name="install-hello-kubectl-cli"></a><span data-ttu-id="f5d23-122">Nainstalujte hello kubectl rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="f5d23-122">Install hello kubectl CLI</span></span>

<span data-ttu-id="f5d23-123">tooconnect toohello Kubernetes clusteru z klientského počítače, použijte [kubectl](https://kubernetes.io/docs/user-guide/kubectl/), hello Kubernetes v klientu příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="f5d23-123">tooconnect toohello Kubernetes cluster from your client computer, use [kubectl](https://kubernetes.io/docs/user-guide/kubectl/), hello Kubernetes command-line client.</span></span> 

<span data-ttu-id="f5d23-124">Pokud používáte Azure CloudShell, `kubectl` je už nainstalován.</span><span class="sxs-lookup"><span data-stu-id="f5d23-124">If you're using Azure CloudShell, `kubectl` is already installed.</span></span> <span data-ttu-id="f5d23-125">Pokud chcete, aby tooinstall ho místně, použijte hello [az acs kubernetes instalace rozhraní příkazového řádku](/cli/azure/acs/kubernetes#install-cli) příkaz.</span><span class="sxs-lookup"><span data-stu-id="f5d23-125">If you want tooinstall it locally, use hello [az acs kubernetes install-cli](/cli/azure/acs/kubernetes#install-cli) command.</span></span>

<span data-ttu-id="f5d23-126">Pokud provozujete v systému Linux nebo systému macOS, může být nutné toorun pomocí příkazu "sudo".</span><span class="sxs-lookup"><span data-stu-id="f5d23-126">If running in Linux or macOS, you may need toorun with sudo.</span></span> <span data-ttu-id="f5d23-127">V systému Windows Ujistěte se, že vaše prostředí byl spuštěn jako správce.</span><span class="sxs-lookup"><span data-stu-id="f5d23-127">On Windows, ensure your shell has been run as administrator.</span></span>

```azurecli-interactive 
az acs kubernetes install-cli 
```

<span data-ttu-id="f5d23-128">V systému Windows, je výchozí instalace hello *c:\program files (x86)\kubectl.exe*.</span><span class="sxs-lookup"><span data-stu-id="f5d23-128">On Windows, hello default installation is *c:\program files (x86)\kubectl.exe*.</span></span> <span data-ttu-id="f5d23-129">Může být nutné tooadd tuto cestu k souboru toohello systému Windows.</span><span class="sxs-lookup"><span data-stu-id="f5d23-129">You may need tooadd this file toohello Windows path.</span></span> 

## <a name="connect-with-kubectl"></a><span data-ttu-id="f5d23-130">Připojení přes kubectl</span><span class="sxs-lookup"><span data-stu-id="f5d23-130">Connect with kubectl</span></span>

<span data-ttu-id="f5d23-131">tooconfigure `kubectl` tooconnect tooyour Kubernetes clusteru, spusťte hello [az acs kubernetes get pověření](/cli/azure/acs/kubernetes#get-credentials) příkaz.</span><span class="sxs-lookup"><span data-stu-id="f5d23-131">tooconfigure `kubectl` tooconnect tooyour Kubernetes cluster, run hello [az acs kubernetes get-credentials](/cli/azure/acs/kubernetes#get-credentials) command.</span></span>

```azurecli-interactive 
az acs kubernetes get-credentials --resource-group=myResourceGroup --name=myK8SCluster
```

<span data-ttu-id="f5d23-132">tooverify hello připojení tooyour clusteru, spusťte hello [kubectl získat uzly](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) příkaz.</span><span class="sxs-lookup"><span data-stu-id="f5d23-132">tooverify hello connection tooyour cluster, run hello [kubectl get nodes](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) command.</span></span>

```azurecli-interactive
kubectl get nodes
```

<span data-ttu-id="f5d23-133">Výstup:</span><span class="sxs-lookup"><span data-stu-id="f5d23-133">Output:</span></span>

```bash
NAME                    STATUS                     AGE       VERSION
k8s-agent-98dc3136-0    Ready                      5m        v1.6.2
k8s-agent-98dc3136-1    Ready                      5m        v1.6.2
k8s-agent-98dc3136-2    Ready                      5m        v1.6.2
k8s-master-98dc3136-0   Ready,SchedulingDisabled   5m        v1.6.2
```

<span data-ttu-id="f5d23-134">V kurzu dokončení máte připravené pro zatížení clusteru služby ACS Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="f5d23-134">At tutorial completion, you have an ACS Kubernetes cluster ready for workloads.</span></span> <span data-ttu-id="f5d23-135">V následujících kurzech je aplikace s více kontejnerů nasazené toothis cluster, škálovat na více systémů, aktualizovat a sledovat.</span><span class="sxs-lookup"><span data-stu-id="f5d23-135">In subsequent tutorials, a multi-container application is deployed toothis cluster, scaled out, updated, and monitored.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f5d23-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f5d23-136">Next steps</span></span>

<span data-ttu-id="f5d23-137">V tomto kurzu byl nasazen clusteru Azure Container Service Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="f5d23-137">In this tutorial, an Azure Container Service Kubernetes cluster was deployed.</span></span> <span data-ttu-id="f5d23-138">byly dokončeny Hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f5d23-138">hello following steps were completed:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f5d23-139">Nasazení clusteru s podporou Kubernetes ACS</span><span class="sxs-lookup"><span data-stu-id="f5d23-139">Deployed a Kubernetes ACS cluster</span></span>
> * <span data-ttu-id="f5d23-140">Nainstalované hello Kubernetes rozhraní příkazového řádku (kubectl)</span><span class="sxs-lookup"><span data-stu-id="f5d23-140">Installed hello Kubernetes CLI (kubectl)</span></span>
> * <span data-ttu-id="f5d23-141">Nakonfigurované kubectl</span><span class="sxs-lookup"><span data-stu-id="f5d23-141">Configured kubectl</span></span>

<span data-ttu-id="f5d23-142">Posunutí další kurz toolearn toohello o spuštění aplikace v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="f5d23-142">Advance toohello next tutorial toolearn about running application on hello cluster.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="f5d23-143">Nasazení aplikace v Kubernetes</span><span class="sxs-lookup"><span data-stu-id="f5d23-143">Deploy application in Kubernetes</span></span>](./container-service-tutorial-kubernetes-deploy-application.md)
