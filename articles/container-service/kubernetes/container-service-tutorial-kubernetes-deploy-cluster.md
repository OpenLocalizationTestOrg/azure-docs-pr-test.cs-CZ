---
title: "Kurz pro Azure Container Service – nasazení clusteru | Microsoft Docs"
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
ms.openlocfilehash: 472697c1f0c18859087d7b448e1786d85c27aca0
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="deploy-a-kubernetes-cluster-in-azure-container-service"></a><span data-ttu-id="46625-104">Nasazení clusteru Kubernetes v Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="46625-104">Deploy a Kubernetes cluster in Azure Container Service</span></span>

<span data-ttu-id="46625-105">Kubernetes poskytuje distribuované platformu pro kontejnerové aplikace.</span><span class="sxs-lookup"><span data-stu-id="46625-105">Kubernetes provides a distributed platform for containerized applications.</span></span> <span data-ttu-id="46625-106">Zřizování clusteru výroby připravené Kubernetes s Azure Container Service je usnadňují a urychlují.</span><span class="sxs-lookup"><span data-stu-id="46625-106">With Azure Container Service, provisioning of a production ready Kubernetes cluster is simple and quick.</span></span> <span data-ttu-id="46625-107">V tomto kurzu, část 3 7, je nasazení clusteru Azure Container Service Kubernetes služby.</span><span class="sxs-lookup"><span data-stu-id="46625-107">In this tutorial, part 3 of 7, an Azure Container Service Kubernetes cluster is deployed.</span></span> <span data-ttu-id="46625-108">Dokončit krokům patří:</span><span class="sxs-lookup"><span data-stu-id="46625-108">Steps completed include:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="46625-109">Nasazení služby ACS Kubernetes clusteru</span><span class="sxs-lookup"><span data-stu-id="46625-109">Deploying a Kubernetes ACS cluster</span></span>
> * <span data-ttu-id="46625-110">Instalace rozhraní příkazového řádku Kubernetes (kubectl)</span><span class="sxs-lookup"><span data-stu-id="46625-110">Installation of the Kubernetes CLI (kubectl)</span></span>
> * <span data-ttu-id="46625-111">Konfigurace kubectl</span><span class="sxs-lookup"><span data-stu-id="46625-111">Configuration of kubectl</span></span>

<span data-ttu-id="46625-112">V následujících kurzech aplikace Azure hlas je nasadit do clusteru, škálovat, aktualizovat a Operations Management Suite je nakonfigurované pro monitorování Kubernetes clusteru.</span><span class="sxs-lookup"><span data-stu-id="46625-112">In subsequent tutorials, the Azure Vote application is deployed to the cluster, scaled, updated, and Operations Management Suite is configured to monitor the Kubernetes cluster.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="46625-113">Než začnete</span><span class="sxs-lookup"><span data-stu-id="46625-113">Before you begin</span></span>

<span data-ttu-id="46625-114">V předchozí kurzy byl bitovou kopii kontejner vytvořit a nahrát do Azure kontejneru registru instance.</span><span class="sxs-lookup"><span data-stu-id="46625-114">In previous tutorials, a container image was created and uploaded to an Azure Container Registry instance.</span></span> <span data-ttu-id="46625-115">Pokud se ještě provést tyto kroky a chcete sledovat, vrátit [kurzu 1 – Vytvoření kontejneru image](./container-service-tutorial-kubernetes-prepare-app.md).</span><span class="sxs-lookup"><span data-stu-id="46625-115">If you have not done these steps, and would like to follow along, return to [Tutorial 1 – Create container images](./container-service-tutorial-kubernetes-prepare-app.md).</span></span>

## <a name="create-kubernetes-cluster"></a><span data-ttu-id="46625-116">Vytvoření clusteru Kubernetes</span><span class="sxs-lookup"><span data-stu-id="46625-116">Create Kubernetes cluster</span></span>

<span data-ttu-id="46625-117">V [předchozí kurzu](./container-service-tutorial-kubernetes-prepare-acr.md), skupinu prostředků s názvem *myResourceGroup* byla vytvořena.</span><span class="sxs-lookup"><span data-stu-id="46625-117">In the [previous tutorial](./container-service-tutorial-kubernetes-prepare-acr.md), a resource group named *myResourceGroup* was created.</span></span> <span data-ttu-id="46625-118">Pokud jste tak dosud neučinili, vytvořte tato skupina prostředků teď.</span><span class="sxs-lookup"><span data-stu-id="46625-118">If you have not done so, create this resource group now.</span></span>

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```

<span data-ttu-id="46625-119">Vytvořte cluster Kubernetes ve službě Azure Container Service pomocí příkazu [az acs create](/cli/azure/acs#create).</span><span class="sxs-lookup"><span data-stu-id="46625-119">Create a Kubernetes cluster in Azure Container Service with the [az acs create](/cli/azure/acs#create) command.</span></span> 

<span data-ttu-id="46625-120">Následující příklad vytvoří cluster s názvem *myK8sCluster* s jedním hlavním linuxovým uzlem a třemi agentskými linuxovými uzly.</span><span class="sxs-lookup"><span data-stu-id="46625-120">The following example creates a cluster named *myK8sCluster* with one Linux master node and three Linux agent nodes.</span></span>

```azurecli-interactive 
az acs create --orchestrator-type=kubernetes --resource-group myResourceGroup --name=myK8SCluster --generate-ssh-keys 
```

<span data-ttu-id="46625-121">Po několika minutách dokončení příkazu a formátu json vrátí informace o nasazení služby ACS.</span><span class="sxs-lookup"><span data-stu-id="46625-121">After several minutes, the command completes, and returns json formatted information about the ACS deployment.</span></span>

## <a name="install-the-kubectl-cli"></a><span data-ttu-id="46625-122">Nainstalujte kubectl rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="46625-122">Install the kubectl CLI</span></span>

<span data-ttu-id="46625-123">Chcete-li připojit ke clusteru Kubernetes z klientského počítače, použijte [kubectl](https://kubernetes.io/docs/user-guide/kubectl/), klient příkazového řádku Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="46625-123">To connect to the Kubernetes cluster from your client computer, use [kubectl](https://kubernetes.io/docs/user-guide/kubectl/), the Kubernetes command-line client.</span></span> 

<span data-ttu-id="46625-124">Pokud používáte Azure CloudShell, `kubectl` je už nainstalován.</span><span class="sxs-lookup"><span data-stu-id="46625-124">If you're using Azure CloudShell, `kubectl` is already installed.</span></span> <span data-ttu-id="46625-125">Pokud chcete nainstalovat místně, použijte [az acs kubernetes instalace rozhraní příkazového řádku](/cli/azure/acs/kubernetes#install-cli) příkaz.</span><span class="sxs-lookup"><span data-stu-id="46625-125">If you want to install it locally, use the [az acs kubernetes install-cli](/cli/azure/acs/kubernetes#install-cli) command.</span></span>

<span data-ttu-id="46625-126">Pokud provozujete v systému Linux nebo systému macOS, můžete spustit pomocí příkazu "sudo".</span><span class="sxs-lookup"><span data-stu-id="46625-126">If running in Linux or macOS, you may need to run with sudo.</span></span> <span data-ttu-id="46625-127">V systému Windows Ujistěte se, že vaše prostředí byl spuštěn jako správce.</span><span class="sxs-lookup"><span data-stu-id="46625-127">On Windows, ensure your shell has been run as administrator.</span></span>

```azurecli-interactive 
az acs kubernetes install-cli 
```

<span data-ttu-id="46625-128">V systému Windows, je výchozí instalace *c:\program files (x86)\kubectl.exe*.</span><span class="sxs-lookup"><span data-stu-id="46625-128">On Windows, the default installation is *c:\program files (x86)\kubectl.exe*.</span></span> <span data-ttu-id="46625-129">Potřebujete přidejte tento soubor do cesty k systému Windows.</span><span class="sxs-lookup"><span data-stu-id="46625-129">You may need to add this file to the Windows path.</span></span> 

## <a name="connect-with-kubectl"></a><span data-ttu-id="46625-130">Připojení přes kubectl</span><span class="sxs-lookup"><span data-stu-id="46625-130">Connect with kubectl</span></span>

<span data-ttu-id="46625-131">Abyste nakonfigurovali `kubectl` pro připojení ke svému clusteru Kubernetes, spusťte příkaz [az acs kubernetes get-credentials](/cli/azure/acs/kubernetes#get-credentials).</span><span class="sxs-lookup"><span data-stu-id="46625-131">To configure `kubectl` to connect to your Kubernetes cluster, run the [az acs kubernetes get-credentials](/cli/azure/acs/kubernetes#get-credentials) command.</span></span>

```azurecli-interactive 
az acs kubernetes get-credentials --resource-group=myResourceGroup --name=myK8SCluster
```

<span data-ttu-id="46625-132">Chcete-li ověřit připojení ke clusteru, spusťte [kubectl získat uzly](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) příkaz.</span><span class="sxs-lookup"><span data-stu-id="46625-132">To verify the connection to your cluster, run the [kubectl get nodes](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) command.</span></span>

```azurecli-interactive
kubectl get nodes
```

<span data-ttu-id="46625-133">Výstup:</span><span class="sxs-lookup"><span data-stu-id="46625-133">Output:</span></span>

```bash
NAME                    STATUS                     AGE       VERSION
k8s-agent-98dc3136-0    Ready                      5m        v1.6.2
k8s-agent-98dc3136-1    Ready                      5m        v1.6.2
k8s-agent-98dc3136-2    Ready                      5m        v1.6.2
k8s-master-98dc3136-0   Ready,SchedulingDisabled   5m        v1.6.2
```

<span data-ttu-id="46625-134">V kurzu dokončení máte připravené pro zatížení clusteru služby ACS Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="46625-134">At tutorial completion, you have an ACS Kubernetes cluster ready for workloads.</span></span> <span data-ttu-id="46625-135">V následujících kurzech aplikace s více kontejnerů je nasadit do tohoto clusteru, škálovat na více systémů, aktualizovat a sledovat.</span><span class="sxs-lookup"><span data-stu-id="46625-135">In subsequent tutorials, a multi-container application is deployed to this cluster, scaled out, updated, and monitored.</span></span>

## <a name="next-steps"></a><span data-ttu-id="46625-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="46625-136">Next steps</span></span>

<span data-ttu-id="46625-137">V tomto kurzu byl nasazen clusteru Azure Container Service Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="46625-137">In this tutorial, an Azure Container Service Kubernetes cluster was deployed.</span></span> <span data-ttu-id="46625-138">Dokončili jste následující kroky:</span><span class="sxs-lookup"><span data-stu-id="46625-138">The following steps were completed:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="46625-139">Nasazení clusteru s podporou Kubernetes ACS</span><span class="sxs-lookup"><span data-stu-id="46625-139">Deployed a Kubernetes ACS cluster</span></span>
> * <span data-ttu-id="46625-140">Nainstalovat rozhraní příkazového řádku Kubernetes (kubectl)</span><span class="sxs-lookup"><span data-stu-id="46625-140">Installed the Kubernetes CLI (kubectl)</span></span>
> * <span data-ttu-id="46625-141">Nakonfigurované kubectl</span><span class="sxs-lookup"><span data-stu-id="46625-141">Configured kubectl</span></span>

<span data-ttu-id="46625-142">Přechodu na v dalším kurzu se dozvíte o spuštění aplikace v clusteru.</span><span class="sxs-lookup"><span data-stu-id="46625-142">Advance to the next tutorial to learn about running application on the cluster.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="46625-143">Nasazení aplikace v Kubernetes</span><span class="sxs-lookup"><span data-stu-id="46625-143">Deploy application in Kubernetes</span></span>](./container-service-tutorial-kubernetes-deploy-application.md)
