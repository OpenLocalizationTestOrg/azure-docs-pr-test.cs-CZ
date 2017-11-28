---
title: aaaQuickstart - Azure Kubernetes clusteru pro Linux | Microsoft Docs
description: "Naučte se rychle toocreate cluster Kubernetes pro Linux kontejnerů v Azure Container Service pomocí hello rozhraní příkazového řádku Azure."
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.assetid: 8da267e8-2aeb-4c24-9a7a-65bdca3a82d6
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/21/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: 8b0d7a803148c1cbf329f4b76f2e99b4b7e14983
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-kubernetes-cluster-for-linux-containers"></a><span data-ttu-id="1d69c-103">Nasazení clusteru Kubernetes pro linuxové kontejnery</span><span class="sxs-lookup"><span data-stu-id="1d69c-103">Deploy Kubernetes cluster for Linux containers</span></span>

<span data-ttu-id="1d69c-104">V této úvodní Kubernetes cluster nasadí pomocí hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="1d69c-104">In this quick start, a Kubernetes cluster is deployed using hello Azure CLI.</span></span> <span data-ttu-id="1d69c-105">Aplikace více kontejneru, který se skládá z webového front-endu a instanci Redis nasazení a poté běží na clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="1d69c-105">A multi-container application consisting of web front end and a Redis instance is then deployed and run on hello cluster.</span></span> <span data-ttu-id="1d69c-106">Po dokončení aplikace hello je přístupné prostřednictvím Internetu hello.</span><span class="sxs-lookup"><span data-stu-id="1d69c-106">Once completed, hello application is accessible over hello internet.</span></span> 

<span data-ttu-id="1d69c-107">Ukázková aplikace Hello v tomto dokumentu je napsán v Python.</span><span class="sxs-lookup"><span data-stu-id="1d69c-107">hello example application used in this document is written in Python.</span></span> <span data-ttu-id="1d69c-108">Koncepty Hello a kroky popsané v tomto poli můžou být použité toodeploy kontejneru, bitové kopie do clusteru s podporou Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="1d69c-108">hello concepts and steps detailed here can be used toodeploy any container image into a Kubernetes cluster.</span></span> <span data-ttu-id="1d69c-109">Hello kód, soubor Docker a předem vytvořené Kubernetes souborů manifestu související toothis projektu jsou k dispozici na [Githubu](https://github.com/Azure-Samples/azure-voting-app-redis.git).</span><span class="sxs-lookup"><span data-stu-id="1d69c-109">hello code, Dockerfile, and pre-created Kubernetes manifest files related toothis project are available on [GitHub](https://github.com/Azure-Samples/azure-voting-app-redis.git).</span></span>

![Obrázek procházení tooAzure hlas](media/container-service-kubernetes-walkthrough/azure-vote.png)

<span data-ttu-id="1d69c-111">Tento úvodní předpokládá základní znalosti koncepce Kubernetes najdete podrobné informace o Kubernetes hello [Kubernetes dokumentaci]( https://kubernetes.io/docs/home/).</span><span class="sxs-lookup"><span data-stu-id="1d69c-111">This quick start assumes a basic understanding of Kubernetes concepts, for detailed information on Kubernetes see hello [Kubernetes documentation]( https://kubernetes.io/docs/home/).</span></span>

<span data-ttu-id="1d69c-112">Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.</span><span class="sxs-lookup"><span data-stu-id="1d69c-112">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="1d69c-113">Pokud zvolíte tooinstall a místně pomocí hello rozhraní příkazového řádku, tento rychlý start vyžaduje, že používáte verzi rozhraní příkazového řádku Azure hello verze 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="1d69c-113">If you choose tooinstall and use hello CLI locally, this quickstart requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="1d69c-114">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="1d69c-114">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="1d69c-115">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="1d69c-115">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="create-a-resource-group"></a><span data-ttu-id="1d69c-116">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="1d69c-116">Create a resource group</span></span>

<span data-ttu-id="1d69c-117">Vytvořte skupinu prostředků s hello [vytvořit skupinu az](/cli/azure/group#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="1d69c-117">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="1d69c-118">Skupina prostředků Azure je logická skupina, ve které se nasazují a spravují prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="1d69c-118">An Azure resource group is a logical group in which Azure resources are deployed and managed.</span></span> 

<span data-ttu-id="1d69c-119">Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *westeurope* umístění.</span><span class="sxs-lookup"><span data-stu-id="1d69c-119">hello following example creates a resource group named *myResourceGroup* in hello *westeurope* location.</span></span>

```azurecli-interactive 
az group create --name myResourceGroup --location westeurope
```

<span data-ttu-id="1d69c-120">Výstup:</span><span class="sxs-lookup"><span data-stu-id="1d69c-120">Output:</span></span>

```json
{
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup",
  "location": "westeurope",
  "managedBy": null,
  "name": "myResourceGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

## <a name="create-kubernetes-cluster"></a><span data-ttu-id="1d69c-121">Vytvoření clusteru Kubernetes</span><span class="sxs-lookup"><span data-stu-id="1d69c-121">Create Kubernetes cluster</span></span>

<span data-ttu-id="1d69c-122">Vytvoření clusteru Kubernetes v Azure Container Service s hello [vytvořit acs az](/cli/azure/acs#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="1d69c-122">Create a Kubernetes cluster in Azure Container Service with hello [az acs create](/cli/azure/acs#create) command.</span></span> <span data-ttu-id="1d69c-123">Hello následující příklad vytvoří cluster s názvem *myK8sCluster* s Linuxem jeden hlavní uzel a tři uzly Linux agent.</span><span class="sxs-lookup"><span data-stu-id="1d69c-123">hello following example creates a cluster named *myK8sCluster* with one Linux master node and three Linux agent nodes.</span></span>

```azurecli-interactive 
az acs create --orchestrator-type kubernetes --resource-group myResourceGroup --name myK8sCluster --generate-ssh-keys 
```

<span data-ttu-id="1d69c-124">Po několika minutách hello příkaz dokončí a vrátí formátu json informace o clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="1d69c-124">After several minutes, hello command completes and returns json formatted information about hello cluster.</span></span> 

## <a name="connect-toohello-cluster"></a><span data-ttu-id="1d69c-125">Připojte toohello cluster</span><span class="sxs-lookup"><span data-stu-id="1d69c-125">Connect toohello cluster</span></span>

<span data-ttu-id="1d69c-126">použít toomanage Kubernetes cluster [kubectl](https://kubernetes.io/docs/user-guide/kubectl/), hello Kubernetes v klientu příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="1d69c-126">toomanage a Kubernetes cluster, use [kubectl](https://kubernetes.io/docs/user-guide/kubectl/), hello Kubernetes command-line client.</span></span> 

<span data-ttu-id="1d69c-127">Pokud používáte Azure Cloud Shell, kubectl je už nainstalován.</span><span class="sxs-lookup"><span data-stu-id="1d69c-127">If you're using Azure CloudShell, kubectl is already installed.</span></span> <span data-ttu-id="1d69c-128">Pokud chcete, aby tooinstall ho místně, můžete použít hello [az acs kubernetes instalace rozhraní příkazového řádku](/cli/azure/acs/kubernetes#install-cli) příkaz.</span><span class="sxs-lookup"><span data-stu-id="1d69c-128">If you want tooinstall it locally, you can use hello [az acs kubernetes install-cli](/cli/azure/acs/kubernetes#install-cli) command.</span></span>

<span data-ttu-id="1d69c-129">tooconfigure kubectl tooconnect tooyour Kubernetes clusteru, spusťte hello [az acs kubernetes get pověření](/cli/azure/acs/kubernetes#get-credentials) příkaz.</span><span class="sxs-lookup"><span data-stu-id="1d69c-129">tooconfigure kubectl tooconnect tooyour Kubernetes cluster, run hello [az acs kubernetes get-credentials](/cli/azure/acs/kubernetes#get-credentials) command.</span></span> <span data-ttu-id="1d69c-130">Tento krok stáhne přihlašovací údaje a nakonfiguruje hello toouse Kubernetes rozhraní příkazového řádku je.</span><span class="sxs-lookup"><span data-stu-id="1d69c-130">This step downloads credentials and configures hello Kubernetes CLI toouse them.</span></span>

```azurecli-interactive 
az acs kubernetes get-credentials --resource-group=myResourceGroup --name=myK8sCluster
```

<span data-ttu-id="1d69c-131">tooverify hello připojení tooyour clusteru, použijte hello [kubectl získat](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) příkaz tooreturn seznam hello uzly clusteru.</span><span class="sxs-lookup"><span data-stu-id="1d69c-131">tooverify hello connection tooyour cluster, use hello [kubectl get](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) command tooreturn a list of hello cluster nodes.</span></span>

```azurecli-interactive
kubectl get nodes
```

<span data-ttu-id="1d69c-132">Výstup:</span><span class="sxs-lookup"><span data-stu-id="1d69c-132">Output:</span></span>

```bash
NAME                    STATUS                     AGE       VERSION
k8s-agent-14ad53a1-0    Ready                      10m       v1.6.6
k8s-agent-14ad53a1-1    Ready                      10m       v1.6.6
k8s-agent-14ad53a1-2    Ready                      10m       v1.6.6
k8s-master-14ad53a1-0   Ready,SchedulingDisabled   10m       v1.6.6
```

## <a name="run-hello-application"></a><span data-ttu-id="1d69c-133">Spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="1d69c-133">Run hello application</span></span>

<span data-ttu-id="1d69c-134">Soubor manifestu Kubernetes definuje požadovaný stav hello clusteru, včetně toho, co by měla být spuštěná kontejneru bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="1d69c-134">A Kubernetes manifest file defines a desired state for hello cluster, including what container images should be running.</span></span> <span data-ttu-id="1d69c-135">V tomto příkladu je manifestu použité toocreate všechny objekty potřeby toorun hello hlas Azure aplikace.</span><span class="sxs-lookup"><span data-stu-id="1d69c-135">For this example, a manifest is used toocreate all objects needed toorun hello Azure Vote application.</span></span> 

<span data-ttu-id="1d69c-136">Vytvořte soubor s názvem `azure-vote.yml` a zkopírujte do něj následující YAML hello.</span><span class="sxs-lookup"><span data-stu-id="1d69c-136">Create a file named `azure-vote.yml` and copy into it hello following YAML.</span></span> <span data-ttu-id="1d69c-137">Pokud pracujete ve službě Azure Cloud Shell, můžete tento soubor vytvořit pomocí editoru vi nebo Nano stejně, jako kdybyste pracovali na virtuálním nebo fyzickém systému.</span><span class="sxs-lookup"><span data-stu-id="1d69c-137">If you are working in Azure Cloud Shell, this file can be created using vi or Nano as if working on a virtual or physical system.</span></span>

```yaml
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: azure-vote-back
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: azure-vote-back
    spec:
      containers:
      - name: azure-vote-back
        image: redis
        ports:
        - containerPort: 6379
          name: redis
---
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-back
spec:
  ports:
  - port: 6379
  selector:
    app: azure-vote-back
---
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: azure-vote-front
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: azure-vote-front
    spec:
      containers:
      - name: azure-vote-front
        image: microsoft/azure-vote-front:redis-v1
        ports:
        - containerPort: 80
        env:
        - name: REDIS
          value: "azure-vote-back"
---
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-front
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-vote-front
```

<span data-ttu-id="1d69c-138">Použití hello [kubectl vytvořit](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#create) příkaz toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="1d69c-138">Use hello [kubectl create](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#create) command toorun hello application.</span></span>

```azurecli-interactive
kubectl create -f azure-vote.yml
```

<span data-ttu-id="1d69c-139">Výstup:</span><span class="sxs-lookup"><span data-stu-id="1d69c-139">Output:</span></span>

```bash
deployment "azure-vote-back" created
service "azure-vote-back" created
deployment "azure-vote-front" created
service "azure-vote-front" created
```

## <a name="test-hello-application"></a><span data-ttu-id="1d69c-140">Testování aplikace hello</span><span class="sxs-lookup"><span data-stu-id="1d69c-140">Test hello application</span></span>

<span data-ttu-id="1d69c-141">Jako hello aplikace běží, [Kubernetes služby](https://kubernetes.io/docs/concepts/services-networking/service/) se vytvoří, zpřístupňuje hello aplikace front-endu toohello Internetu.</span><span class="sxs-lookup"><span data-stu-id="1d69c-141">As hello application is run, a [Kubernetes service](https://kubernetes.io/docs/concepts/services-networking/service/) is created that exposes hello application front end toohello internet.</span></span> <span data-ttu-id="1d69c-142">Tento proces může trvat několik minut toocomplete.</span><span class="sxs-lookup"><span data-stu-id="1d69c-142">This process can take a few minutes toocomplete.</span></span> 

<span data-ttu-id="1d69c-143">toomonitor průběh, použijte hello [kubectl získat služby](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) s hello `--watch` argument.</span><span class="sxs-lookup"><span data-stu-id="1d69c-143">toomonitor progress, use hello [kubectl get service](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) command with hello `--watch` argument.</span></span>

```azurecli-interactive
kubectl get service azure-vote-front --watch
```

<span data-ttu-id="1d69c-144">Původně hello **externí IP** pro hello *azure hlas front* služby se zobrazí jako *čekající*.</span><span class="sxs-lookup"><span data-stu-id="1d69c-144">Initially hello **EXTERNAL-IP** for hello *azure-vote-front* service appears as *pending*.</span></span> <span data-ttu-id="1d69c-145">Jakmile hello externí IP adresu se změnil z *čekající* tooan *IP adresu*, použijte `CTRL-C` toostop hello kubectl sledovat proces.</span><span class="sxs-lookup"><span data-stu-id="1d69c-145">Once hello EXTERNAL-IP address has changed from *pending* tooan *IP address*, use `CTRL-C` toostop hello kubectl watch process.</span></span> 
  
```bash
azure-vote-front   10.0.34.242   <pending>     80:30676/TCP   7s
azure-vote-front   10.0.34.242   52.179.23.131   80:30676/TCP   2m
```

<span data-ttu-id="1d69c-146">Nyní je možné procházet toohello externí IP adresu toosee hello hlas aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="1d69c-146">You can now browse toohello external IP address toosee hello Azure Vote App.</span></span>

![Obrázek procházení tooAzure hlas](media/container-service-kubernetes-walkthrough/azure-vote.png)  

## <a name="delete-cluster"></a><span data-ttu-id="1d69c-148">Odstranění clusteru</span><span class="sxs-lookup"><span data-stu-id="1d69c-148">Delete cluster</span></span>
<span data-ttu-id="1d69c-149">Pokud hello cluster je již nepotřebujete, můžete použít hello [odstranění skupiny az](/cli/azure/group#delete) příkaz skupiny prostředků hello tooremove, container service a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="1d69c-149">When hello cluster is no longer needed, you can use hello [az group delete](/cli/azure/group#delete) command tooremove hello resource group, container service, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup --yes --no-wait
```

## <a name="get-hello-code"></a><span data-ttu-id="1d69c-150">Získat kód hello</span><span class="sxs-lookup"><span data-stu-id="1d69c-150">Get hello code</span></span>

<span data-ttu-id="1d69c-151">V této úvodní předem vytvořené kontejneru bitové kopie byly použité toocreate Kubernetes nasazení.</span><span class="sxs-lookup"><span data-stu-id="1d69c-151">In this quick start, pre-created container images have been used toocreate a Kubernetes deployment.</span></span> <span data-ttu-id="1d69c-152">Hello související kód aplikace, soubor Docker, a soubor manifestu Kubernetes jsou dostupné na Githubu.</span><span class="sxs-lookup"><span data-stu-id="1d69c-152">hello related application code, Dockerfile, and Kubernetes manifest file are available on GitHub.</span></span>

[<span data-ttu-id="1d69c-153">https://github.com/Azure-Samples/azure-voting-app-redis</span><span class="sxs-lookup"><span data-stu-id="1d69c-153">https://github.com/Azure-Samples/azure-voting-app-redis</span></span>](https://github.com/Azure-Samples/azure-voting-app-redis.git)

## <a name="next-steps"></a><span data-ttu-id="1d69c-154">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1d69c-154">Next steps</span></span>

<span data-ttu-id="1d69c-155">V této úvodní nasazení clusteru s podporou Kubernetes a nasadit aplikace s více kontejnerů tooit.</span><span class="sxs-lookup"><span data-stu-id="1d69c-155">In this quick start, you deployed a Kubernetes cluster and deployed a multi-container application tooit.</span></span> 

<span data-ttu-id="1d69c-156">toolearn informace o Azure Container Service a pomocí příklad toodeployment dokončení kódu, pokračovat toohello Kubernetes clusteru kurzu.</span><span class="sxs-lookup"><span data-stu-id="1d69c-156">toolearn more about Azure Container Service, and walk through a complete code toodeployment example, continue toohello Kubernetes cluster tutorial.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="1d69c-157">Správa clusteru ACS Kubernetes</span><span class="sxs-lookup"><span data-stu-id="1d69c-157">Manage an ACS Kubernetes cluster</span></span>](./container-service-tutorial-kubernetes-prepare-app.md)
