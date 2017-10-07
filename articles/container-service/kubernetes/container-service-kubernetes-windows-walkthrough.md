---
title: aaaQuickstart - clusteru Azure Kubernetes pro Windows | Microsoft Docs
description: "Naučte se rychle toocreate Kubernetes clusteru Windows kontejnerů v Azure Container Service s hello rozhraní příkazového řádku Azure."
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: 85fe65a46ae8c78797e8a8a097c2a37f06329335
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-kubernetes-cluster-for-windows-containers"></a><span data-ttu-id="0e609-103">Nasazení clusteru Kubernetes pro kontejnery Windows</span><span class="sxs-lookup"><span data-stu-id="0e609-103">Deploy Kubernetes cluster for Windows containers</span></span>

<span data-ttu-id="0e609-104">Hello rozhraní příkazového řádku Azure je použité toocreate a spravovat prostředky Azure z hello příkazového řádku nebo ve skriptech.</span><span class="sxs-lookup"><span data-stu-id="0e609-104">hello Azure CLI is used toocreate and manage Azure resources from hello command line or in scripts.</span></span> <span data-ttu-id="0e609-105">Tato příručka podrobně popisuje pomocí rozhraní příkazového řádku Azure toodeploy hello [Kubernetes](https://kubernetes.io/docs/home/) clusteru v [Azure Container Service](../container-service-intro.md).</span><span class="sxs-lookup"><span data-stu-id="0e609-105">This guide details using hello Azure CLI toodeploy a [Kubernetes](https://kubernetes.io/docs/home/) cluster in [Azure Container Service](../container-service-intro.md).</span></span> <span data-ttu-id="0e609-106">Po nasazení clusteru hello připojíte tooit s hello Kubernetes `kubectl` nástroj příkazového řádku a nasadit vaši první kontejneru systému Windows.</span><span class="sxs-lookup"><span data-stu-id="0e609-106">Once hello cluster is deployed, you connect tooit with hello Kubernetes `kubectl` command-line tool, and you deploy your first Windows container.</span></span>

<span data-ttu-id="0e609-107">Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.</span><span class="sxs-lookup"><span data-stu-id="0e609-107">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="0e609-108">Pokud zvolíte tooinstall a místně pomocí hello rozhraní příkazového řádku, tento rychlý start vyžaduje, že používáte verzi rozhraní příkazového řádku Azure hello verze 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="0e609-108">If you choose tooinstall and use hello CLI locally, this quickstart requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="0e609-109">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="0e609-109">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="0e609-110">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="0e609-110">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

> [!NOTE]
> <span data-ttu-id="0e609-111">Podpora pro kontejnery Windows v Kubernetes ve službě Azure Container Service je ve verzi Preview.</span><span class="sxs-lookup"><span data-stu-id="0e609-111">Support for Windows containers on Kubernetes in Azure Container Service is in preview.</span></span> 
>

## <a name="create-a-resource-group"></a><span data-ttu-id="0e609-112">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="0e609-112">Create a resource group</span></span>

<span data-ttu-id="0e609-113">Vytvořte skupinu prostředků s hello [vytvořit skupinu az](/cli/azure/group#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="0e609-113">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="0e609-114">Skupina prostředků Azure je logická skupina, ve které se nasazují a spravují prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="0e609-114">An Azure resource group is a logical group in which Azure resources are deployed and managed.</span></span> 

<span data-ttu-id="0e609-115">Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *eastus* umístění.</span><span class="sxs-lookup"><span data-stu-id="0e609-115">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location.</span></span>

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-kubernetes-cluster"></a><span data-ttu-id="0e609-116">Vytvoření clusteru Kubernetes</span><span class="sxs-lookup"><span data-stu-id="0e609-116">Create Kubernetes cluster</span></span>
<span data-ttu-id="0e609-117">Vytvoření clusteru Kubernetes v Azure Container Service s hello [vytvořit acs az](/cli/azure/acs#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="0e609-117">Create a Kubernetes cluster in Azure Container Service with hello [az acs create](/cli/azure/acs#create) command.</span></span> 

<span data-ttu-id="0e609-118">Hello následující příklad vytvoří cluster s názvem *myK8sCluster* s Linuxem jeden hlavní uzel a dva uzly agenta systému Windows.</span><span class="sxs-lookup"><span data-stu-id="0e609-118">hello following example creates a cluster named *myK8sCluster* with one Linux master node and two Windows agent nodes.</span></span> <span data-ttu-id="0e609-119">Tento příklad vytvoří hlavní Linux toohello potřebné tooconnect klíče SSH.</span><span class="sxs-lookup"><span data-stu-id="0e609-119">This example creates SSH keys needed tooconnect toohello Linux master.</span></span> <span data-ttu-id="0e609-120">Tento příklad používá *azureuser* pro název správce a *myPassword12* jako heslo hello na uzlech Windows hello.</span><span class="sxs-lookup"><span data-stu-id="0e609-120">This example uses *azureuser* for an administrative user name and *myPassword12* as hello password on hello Windows nodes.</span></span> <span data-ttu-id="0e609-121">Aktualizujte tyto hodnoty toosomething odpovídající tooyour prostředí.</span><span class="sxs-lookup"><span data-stu-id="0e609-121">Update these values toosomething appropriate tooyour environment.</span></span> 



```azurecli-interactive 
az acs create --orchestrator-type=kubernetes \
    --resource-group myResourceGroup \
    --name=myK8sCluster \
    --agent-count=2 \
    --generate-ssh-keys \
    --windows --admin-username azureuser \
    --admin-password myPassword12
```

<span data-ttu-id="0e609-122">Po několika minutách hello příkaz dokončí a zobrazí informace o nasazení.</span><span class="sxs-lookup"><span data-stu-id="0e609-122">After several minutes, hello command completes, and shows you information about your deployment.</span></span>

## <a name="install-kubectl"></a><span data-ttu-id="0e609-123">Instalace kubectl</span><span class="sxs-lookup"><span data-stu-id="0e609-123">Install kubectl</span></span>

<span data-ttu-id="0e609-124">tooconnect toohello Kubernetes clusteru z klientského počítače, použijte [ `kubectl` ](https://kubernetes.io/docs/user-guide/kubectl/), hello Kubernetes v klientu příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="0e609-124">tooconnect toohello Kubernetes cluster from your client computer, use [`kubectl`](https://kubernetes.io/docs/user-guide/kubectl/), hello Kubernetes command-line client.</span></span> 

<span data-ttu-id="0e609-125">Pokud používáte Azure CloudShell, `kubectl` je už nainstalován.</span><span class="sxs-lookup"><span data-stu-id="0e609-125">If you're using Azure CloudShell, `kubectl` is already installed.</span></span> <span data-ttu-id="0e609-126">Pokud chcete, aby tooinstall ho místně, můžete použít hello [az acs kubernetes instalace rozhraní příkazového řádku](/cli/azure/acs/kubernetes#install-cli) příkaz.</span><span class="sxs-lookup"><span data-stu-id="0e609-126">If you want tooinstall it locally, you can use hello [az acs kubernetes install-cli](/cli/azure/acs/kubernetes#install-cli) command.</span></span>

<span data-ttu-id="0e609-127">Následující příklad nainstaluje rozhraní příkazového řádku Azure Hello `kubectl` tooyour systému.</span><span class="sxs-lookup"><span data-stu-id="0e609-127">hello following Azure CLI example installs `kubectl` tooyour system.</span></span> <span data-ttu-id="0e609-128">V systému Windows tento příkaz spusťte jako správce.</span><span class="sxs-lookup"><span data-stu-id="0e609-128">On Windows, run this command as an administrator.</span></span>

```azurecli-interactive 
az acs kubernetes install-cli
```


## <a name="connect-with-kubectl"></a><span data-ttu-id="0e609-129">Připojení přes kubectl</span><span class="sxs-lookup"><span data-stu-id="0e609-129">Connect with kubectl</span></span>

<span data-ttu-id="0e609-130">tooconfigure `kubectl` tooconnect tooyour Kubernetes clusteru, spusťte hello [az acs kubernetes get pověření](/cli/azure/acs/kubernetes#get-credentials) příkaz.</span><span class="sxs-lookup"><span data-stu-id="0e609-130">tooconfigure `kubectl` tooconnect tooyour Kubernetes cluster, run hello [az acs kubernetes get-credentials](/cli/azure/acs/kubernetes#get-credentials) command.</span></span> <span data-ttu-id="0e609-131">Hello následující příklad stáhne hello konfigurace clusteru pro váš cluster Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="0e609-131">hello following example downloads hello cluster configuration for your Kubernetes cluster.</span></span>

```azurecli-interactive 
az acs kubernetes get-credentials --resource-group=myResourceGroup --name=myK8sCluster
```

<span data-ttu-id="0e609-132">tooverify hello připojení tooyour clusteru z počítače, zkuste spustit:</span><span class="sxs-lookup"><span data-stu-id="0e609-132">tooverify hello connection tooyour cluster from your machine, try running:</span></span>

```azurecli-interactive
kubectl get nodes
```

<span data-ttu-id="0e609-133">`kubectl`uvádí hello hlavní server a agenta uzly.</span><span class="sxs-lookup"><span data-stu-id="0e609-133">`kubectl` lists hello master and agent nodes.</span></span>

```azurecli-interactive
NAME                    STATUS                     AGE       VERSION
k8s-agent-98dc3136-0    Ready                      5m        v1.5.3
k8s-agent-98dc3136-1    Ready                      5m        v1.5.3
k8s-master-98dc3136-0   Ready,SchedulingDisabled   5m        v1.5.3

```

## <a name="deploy-a-windows-iis-container"></a><span data-ttu-id="0e609-134">Nasazení kontejneru Windows služby IIS</span><span class="sxs-lookup"><span data-stu-id="0e609-134">Deploy a Windows IIS container</span></span>

<span data-ttu-id="0e609-135">Kontejner Dockeru můžete spustit v *podu* Kubernetes, který obsahuje jeden nebo více kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="0e609-135">You can run a Docker container inside a Kubernetes *pod*, which contains one or more containers.</span></span> 

<span data-ttu-id="0e609-136">Tato základní příklad používá toospecify soubor JSON kontejner Microsoft Internet Information Server (IIS) a potom vytvoří hello pod pomocí hello `kubctl apply` příkaz.</span><span class="sxs-lookup"><span data-stu-id="0e609-136">This basic example uses a JSON file toospecify a Microsoft Internet Information Server (IIS) container, and then creates hello pod using hello `kubctl apply` command.</span></span> 

<span data-ttu-id="0e609-137">Vytvořte místní soubor s názvem `iis.json` a kopírování hello následující text.</span><span class="sxs-lookup"><span data-stu-id="0e609-137">Create a local file named `iis.json` and copy hello following text.</span></span> <span data-ttu-id="0e609-138">Tento soubor informuje Kubernetes toorun služby IIS v systému Windows Server 2016 Nano Server pomocí bitové kopie veřejného kontejneru z [úložiště Docker Hub](https://hub.docker.com/r/nanoserver/iis/).</span><span class="sxs-lookup"><span data-stu-id="0e609-138">This file tells Kubernetes toorun IIS on Windows Server 2016 Nano Server, using a public container image from [Docker Hub](https://hub.docker.com/r/nanoserver/iis/).</span></span> <span data-ttu-id="0e609-139">kontejner Hello používá port 80, ale zpočátku je k dispozici pouze v rámci sítě clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="0e609-139">hello container uses port 80, but initially is only accessible within hello cluster network.</span></span>

 ```JSON
 {
  "apiVersion": "v1",
  "kind": "Pod",
  "metadata": {
    "name": "iis",
    "labels": {
      "name": "iis"
    }
  },
  "spec": {
    "containers": [
      {
        "name": "iis",
        "image": "nanoserver/iis",
        "ports": [
          {
          "containerPort": 80
          }
        ]
      }
    ],
    "nodeSelector": {
     "beta.kubernetes.io/os": "windows"
     }
   }
 }
 ```

<span data-ttu-id="0e609-140">toostart hello pod, typ:</span><span class="sxs-lookup"><span data-stu-id="0e609-140">toostart hello pod, type:</span></span>
  
```azurecli-interactive
kubectl apply -f iis.json
```  

<span data-ttu-id="0e609-141">tootrack hello nasazení, typ:</span><span class="sxs-lookup"><span data-stu-id="0e609-141">tootrack hello deployment, type:</span></span>
  
```azurecli-interactive
kubectl get pods
```

<span data-ttu-id="0e609-142">Hello pod je nasazení, i když je stav hello `ContainerCreating`.</span><span class="sxs-lookup"><span data-stu-id="0e609-142">While hello pod is deploying, hello status is `ContainerCreating`.</span></span> <span data-ttu-id="0e609-143">Může trvat několik minut, než hello kontejneru tooenter hello `Running` stavu.</span><span class="sxs-lookup"><span data-stu-id="0e609-143">It can take a few minutes for hello container tooenter hello `Running` state.</span></span>

```azurecli-interactive
NAME     READY        STATUS        RESTARTS    AGE
iis      1/1          Running       0           32s
```

## <a name="view-hello-iis-welcome-page"></a><span data-ttu-id="0e609-144">Zobrazení hello úvodní stránka služby IIS</span><span class="sxs-lookup"><span data-stu-id="0e609-144">View hello IIS welcome page</span></span>

<span data-ttu-id="0e609-145">tooexpose pod toohello Vítáme s veřejnou IP adresu, typ hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="0e609-145">tooexpose hello pod toohello world with a public IP address, type hello following command:</span></span>

```azurecli-interactive
kubectl expose pods iis --port=80 --type=LoadBalancer
```

<span data-ttu-id="0e609-146">Pomocí tohoto příkazu Kubernetes vytvoří službu a [pravidlo nástroje pro vyrovnávání zatížení Azure](container-service-kubernetes-load-balancing.md) s veřejnou IP adresu pro službu hello.</span><span class="sxs-lookup"><span data-stu-id="0e609-146">With this command, Kubernetes creates a service and an [Azure load balancer rule](container-service-kubernetes-load-balancing.md) with a public IP address for hello service.</span></span> 

<span data-ttu-id="0e609-147">Spusťte následující příkaz toosee hello stav služby hello hello.</span><span class="sxs-lookup"><span data-stu-id="0e609-147">Run hello following command toosee hello status of hello service.</span></span>

```azurecli-interactive
kubectl get svc
```

<span data-ttu-id="0e609-148">Původně hello IP adresa se zobrazí jako `pending`.</span><span class="sxs-lookup"><span data-stu-id="0e609-148">Initially hello IP address appears as `pending`.</span></span> <span data-ttu-id="0e609-149">Po několika minutách hello externí IP adresu hello `iis` pod nastavena:</span><span class="sxs-lookup"><span data-stu-id="0e609-149">After a few minutes, hello external IP address of hello `iis` pod is set:</span></span>
  
```azurecli-interactive
NAME         CLUSTER-IP     EXTERNAL-IP     PORT(S)        AGE       
kubernetes   10.0.0.1       <none>          443/TCP        21h       
iis          10.0.111.25    13.64.158.233   80/TCP         22m
```

<span data-ttu-id="0e609-150">Můžete použít webový prohlížeč choice toosee hello výchozí IIS úvodní stránky v hello externí IP adresu:</span><span class="sxs-lookup"><span data-stu-id="0e609-150">You can use a web browser of your choice toosee hello default IIS welcome page at hello external IP address:</span></span>

![Obrázek procházení tooIIS](./media/container-service-kubernetes-windows-walkthrough/kubernetes-iis.png)  


## <a name="delete-cluster"></a><span data-ttu-id="0e609-152">Odstranění clusteru</span><span class="sxs-lookup"><span data-stu-id="0e609-152">Delete cluster</span></span>
<span data-ttu-id="0e609-153">Pokud hello cluster je již nepotřebujete, můžete použít hello [odstranění skupiny az](/cli/azure/group#delete) příkaz skupiny prostředků hello tooremove, container service a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="0e609-153">When hello cluster is no longer needed, you can use hello [az group delete](/cli/azure/group#delete) command tooremove hello resource group, container service, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```


## <a name="next-steps"></a><span data-ttu-id="0e609-154">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0e609-154">Next steps</span></span>

<span data-ttu-id="0e609-155">V tomto rychlém úvodním kurzu jste nasadili cluster Kubernetes, připojili se přes `kubectl` a nasadili pod s kontejnerem ISS.</span><span class="sxs-lookup"><span data-stu-id="0e609-155">In this quick start, you deployed a Kubernetes cluster, connected with `kubectl`, and deployed a pod with an IIS container.</span></span> <span data-ttu-id="0e609-156">toolearn Další informace o Azure Container Service pokračovat toohello Kubernetes kurzu.</span><span class="sxs-lookup"><span data-stu-id="0e609-156">toolearn more about Azure Container Service, continue toohello Kubernetes tutorial.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="0e609-157">Správa clusteru ACS Kubernetes</span><span class="sxs-lookup"><span data-stu-id="0e609-157">Manage an ACS Kubernetes cluster</span></span>](container-service-tutorial-kubernetes-prepare-app.md)
