---
title: "Rychlý start – Azure Kubernetes cluster pro Windows | Dokumentace Microsoftu"
description: "Rychle se naučíte, jak pomocí rozhraní příkazového řádku Azure vytvářet cluster Kubernetes pro kontejnery Windows ve službě Azure Container Service."
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
ms.openlocfilehash: f9bf4c4094addfa9654e3b99d91add03079ee045
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="deploy-kubernetes-cluster-for-windows-containers"></a><span data-ttu-id="504c1-103">Nasazení clusteru Kubernetes pro kontejnery Windows</span><span class="sxs-lookup"><span data-stu-id="504c1-103">Deploy Kubernetes cluster for Windows containers</span></span>

<span data-ttu-id="504c1-104">Azure CLI slouží k vytváření a správě prostředků Azure z příkazového řádku nebo ve skriptech.</span><span class="sxs-lookup"><span data-stu-id="504c1-104">The Azure CLI is used to create and manage Azure resources from the command line or in scripts.</span></span> <span data-ttu-id="504c1-105">Tato příručka podrobně popisuje použití rozhraní příkazového řádku Azure k nasazení clusteru [Kubernetes](https://kubernetes.io/docs/home/) ve službě [Azure Container Service](../container-service-intro.md).</span><span class="sxs-lookup"><span data-stu-id="504c1-105">This guide details using the Azure CLI to deploy a [Kubernetes](https://kubernetes.io/docs/home/) cluster in [Azure Container Service](../container-service-intro.md).</span></span> <span data-ttu-id="504c1-106">Po nasazení clusteru se k němu připojíte pomocí nástroje pro příkazový řádek Kubernetes `kubectl` a nasadíte svůj první kontejner Windows.</span><span class="sxs-lookup"><span data-stu-id="504c1-106">Once the cluster is deployed, you connect to it with the Kubernetes `kubectl` command-line tool, and you deploy your first Windows container.</span></span>

<span data-ttu-id="504c1-107">Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.</span><span class="sxs-lookup"><span data-stu-id="504c1-107">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="504c1-108">Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku místně, musíte mít rozhraní příkazového řádku Azure ve verzi 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="504c1-108">If you choose to install and use the CLI locally, this quickstart requires that you are running the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="504c1-109">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="504c1-109">Run `az --version` to find the version.</span></span> <span data-ttu-id="504c1-110">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="504c1-110">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

> [!NOTE]
> <span data-ttu-id="504c1-111">Podpora pro kontejnery Windows v Kubernetes ve službě Azure Container Service je ve verzi Preview.</span><span class="sxs-lookup"><span data-stu-id="504c1-111">Support for Windows containers on Kubernetes in Azure Container Service is in preview.</span></span> 
>

## <a name="create-a-resource-group"></a><span data-ttu-id="504c1-112">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="504c1-112">Create a resource group</span></span>

<span data-ttu-id="504c1-113">Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="504c1-113">Create a resource group with the [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="504c1-114">Skupina prostředků Azure je logická skupina, ve které se nasazují a spravují prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="504c1-114">An Azure resource group is a logical group in which Azure resources are deployed and managed.</span></span> 

<span data-ttu-id="504c1-115">Následující příklad vytvoří skupinu prostředků *myResourceGroup* v umístění *eastus*.</span><span class="sxs-lookup"><span data-stu-id="504c1-115">The following example creates a resource group named *myResourceGroup* in the *eastus* location.</span></span>

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-kubernetes-cluster"></a><span data-ttu-id="504c1-116">Vytvoření clusteru Kubernetes</span><span class="sxs-lookup"><span data-stu-id="504c1-116">Create Kubernetes cluster</span></span>
<span data-ttu-id="504c1-117">Vytvořte cluster Kubernetes ve službě Azure Container Service pomocí příkazu [az acs create](/cli/azure/acs#create).</span><span class="sxs-lookup"><span data-stu-id="504c1-117">Create a Kubernetes cluster in Azure Container Service with the [az acs create](/cli/azure/acs#create) command.</span></span> 

<span data-ttu-id="504c1-118">Následující příklad vytvoří cluster s názvem *myK8sCluster* s jedním hlavním linuxovým uzlem a dvěma agentskými uzly Windows.</span><span class="sxs-lookup"><span data-stu-id="504c1-118">The following example creates a cluster named *myK8sCluster* with one Linux master node and two Windows agent nodes.</span></span> <span data-ttu-id="504c1-119">Tento příklad vytvoří klíče SSH potřebné pro připojení k hlavnímu serveru Linux.</span><span class="sxs-lookup"><span data-stu-id="504c1-119">This example creates SSH keys needed to connect to the Linux master.</span></span> <span data-ttu-id="504c1-120">Tento příklad používá na uzlech Windows uživatelské jméno správce *azureuser* a heslo *myPassword12*.</span><span class="sxs-lookup"><span data-stu-id="504c1-120">This example uses *azureuser* for an administrative user name and *myPassword12* as the password on the Windows nodes.</span></span> <span data-ttu-id="504c1-121">Aktualizujte tyto hodnoty na nějaké vhodné pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="504c1-121">Update these values to something appropriate to your environment.</span></span> 



```azurecli-interactive 
az acs create --orchestrator-type=kubernetes \
    --resource-group myResourceGroup \
    --name=myK8sCluster \
    --agent-count=2 \
    --generate-ssh-keys \
    --windows --admin-username azureuser \
    --admin-password myPassword12
```

<span data-ttu-id="504c1-122">Po několika minutách se příkaz dokončí a zobrazí vám informace o nasazení.</span><span class="sxs-lookup"><span data-stu-id="504c1-122">After several minutes, the command completes, and shows you information about your deployment.</span></span>

## <a name="install-kubectl"></a><span data-ttu-id="504c1-123">Instalace kubectl</span><span class="sxs-lookup"><span data-stu-id="504c1-123">Install kubectl</span></span>

<span data-ttu-id="504c1-124">Pokud se chcete připojit ke clusteru Kubernetes z klientského počítače, použijte klienta příkazového řádku Kubernetes [`kubectl`](https://kubernetes.io/docs/user-guide/kubectl/).</span><span class="sxs-lookup"><span data-stu-id="504c1-124">To connect to the Kubernetes cluster from your client computer, use [`kubectl`](https://kubernetes.io/docs/user-guide/kubectl/), the Kubernetes command-line client.</span></span> 

<span data-ttu-id="504c1-125">Pokud používáte Azure CloudShell, `kubectl` je už nainstalován.</span><span class="sxs-lookup"><span data-stu-id="504c1-125">If you're using Azure CloudShell, `kubectl` is already installed.</span></span> <span data-ttu-id="504c1-126">Pokud ho chcete nainstalovat místně, můžete použít příkaz [az acs kubernetes install-cli](/cli/azure/acs/kubernetes#install-cli).</span><span class="sxs-lookup"><span data-stu-id="504c1-126">If you want to install it locally, you can use the [az acs kubernetes install-cli](/cli/azure/acs/kubernetes#install-cli) command.</span></span>

<span data-ttu-id="504c1-127">Následující příklad rozhraní příkazového řádku Azure nainstaluje `kubectl` do vašeho systému.</span><span class="sxs-lookup"><span data-stu-id="504c1-127">The following Azure CLI example installs `kubectl` to your system.</span></span> <span data-ttu-id="504c1-128">V systému Windows tento příkaz spusťte jako správce.</span><span class="sxs-lookup"><span data-stu-id="504c1-128">On Windows, run this command as an administrator.</span></span>

```azurecli-interactive 
az acs kubernetes install-cli
```


## <a name="connect-with-kubectl"></a><span data-ttu-id="504c1-129">Připojení přes kubectl</span><span class="sxs-lookup"><span data-stu-id="504c1-129">Connect with kubectl</span></span>

<span data-ttu-id="504c1-130">Abyste nakonfigurovali `kubectl` pro připojení ke svému clusteru Kubernetes, spusťte příkaz [az acs kubernetes get-credentials](/cli/azure/acs/kubernetes#get-credentials).</span><span class="sxs-lookup"><span data-stu-id="504c1-130">To configure `kubectl` to connect to your Kubernetes cluster, run the [az acs kubernetes get-credentials](/cli/azure/acs/kubernetes#get-credentials) command.</span></span> <span data-ttu-id="504c1-131">Následující příklad stáhne konfiguraci clusteru pro cluster Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="504c1-131">The following example downloads the cluster configuration for your Kubernetes cluster.</span></span>

```azurecli-interactive 
az acs kubernetes get-credentials --resource-group=myResourceGroup --name=myK8sCluster
```

<span data-ttu-id="504c1-132">Abyste ověřili připojení ke clusteru ze svého počítače, zkuste spustit:</span><span class="sxs-lookup"><span data-stu-id="504c1-132">To verify the connection to your cluster from your machine, try running:</span></span>

```azurecli-interactive
kubectl get nodes
```

<span data-ttu-id="504c1-133">`kubectl` zobrazuje seznam hlavních a agentských uzlů.</span><span class="sxs-lookup"><span data-stu-id="504c1-133">`kubectl` lists the master and agent nodes.</span></span>

```azurecli-interactive
NAME                    STATUS                     AGE       VERSION
k8s-agent-98dc3136-0    Ready                      5m        v1.5.3
k8s-agent-98dc3136-1    Ready                      5m        v1.5.3
k8s-master-98dc3136-0   Ready,SchedulingDisabled   5m        v1.5.3

```

## <a name="deploy-a-windows-iis-container"></a><span data-ttu-id="504c1-134">Nasazení kontejneru Windows služby IIS</span><span class="sxs-lookup"><span data-stu-id="504c1-134">Deploy a Windows IIS container</span></span>

<span data-ttu-id="504c1-135">Kontejner Dockeru můžete spustit v *podu* Kubernetes, který obsahuje jeden nebo více kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="504c1-135">You can run a Docker container inside a Kubernetes *pod*, which contains one or more containers.</span></span> 

<span data-ttu-id="504c1-136">Tento základní příklad používá soubor JSON k určení kontejneru se serverem služby Microsoft IIS a pak vytvoří pod pomocí příkazu `kubctl apply`.</span><span class="sxs-lookup"><span data-stu-id="504c1-136">This basic example uses a JSON file to specify a Microsoft Internet Information Server (IIS) container, and then creates the pod using the `kubctl apply` command.</span></span> 

<span data-ttu-id="504c1-137">Vytvořte místní soubor `iis.json` a zkopírujte do něj následující text.</span><span class="sxs-lookup"><span data-stu-id="504c1-137">Create a local file named `iis.json` and copy the following text.</span></span> <span data-ttu-id="504c1-138">Tento soubor říká Kubernetes, aby spustil službu IIS v systému Windows Server 2016 Nano Server s použitím veřejné image kontejneru z [Docker Hubu](https://hub.docker.com/r/nanoserver/iis/).</span><span class="sxs-lookup"><span data-stu-id="504c1-138">This file tells Kubernetes to run IIS on Windows Server 2016 Nano Server, using a public container image from [Docker Hub](https://hub.docker.com/r/nanoserver/iis/).</span></span> <span data-ttu-id="504c1-139">Kontejner používá port 80, ale zpočátku je přístupný pouze v rámci sítě s clustery.</span><span class="sxs-lookup"><span data-stu-id="504c1-139">The container uses port 80, but initially is only accessible within the cluster network.</span></span>

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

<span data-ttu-id="504c1-140">Pokud chcete pod spustit, zadejte:</span><span class="sxs-lookup"><span data-stu-id="504c1-140">To start the pod, type:</span></span>
  
```azurecli-interactive
kubectl apply -f iis.json
```  

<span data-ttu-id="504c1-141">Pokud chcete sledovat nasazení, zadejte:</span><span class="sxs-lookup"><span data-stu-id="504c1-141">To track the deployment, type:</span></span>
  
```azurecli-interactive
kubectl get pods
```

<span data-ttu-id="504c1-142">Zatímco se pod nasazuje, je jeho stav `ContainerCreating` (Vytváření kontejneru).</span><span class="sxs-lookup"><span data-stu-id="504c1-142">While the pod is deploying, the status is `ContainerCreating`.</span></span> <span data-ttu-id="504c1-143">Přechod kontejneru do stavu `Running` (Spuštěný) může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="504c1-143">It can take a few minutes for the container to enter the `Running` state.</span></span>

```azurecli-interactive
NAME     READY        STATUS        RESTARTS    AGE
iis      1/1          Running       0           32s
```

## <a name="view-the-iis-welcome-page"></a><span data-ttu-id="504c1-144">Zobrazení úvodní stránky služby IIS</span><span class="sxs-lookup"><span data-stu-id="504c1-144">View the IIS welcome page</span></span>

<span data-ttu-id="504c1-145">Pokud chcete zpřístupnit pod celému světu prostřednictvím veřejné IP adresy, zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="504c1-145">To expose the pod to the world with a public IP address, type the following command:</span></span>

```azurecli-interactive
kubectl expose pods iis --port=80 --type=LoadBalancer
```

<span data-ttu-id="504c1-146">Tento příkaz způsobí, že Kubernetes vytvoří službu a [pravidlo nástroje pro vyrovnávání zatížení Azure](container-service-kubernetes-load-balancing.md) s veřejnou IP adresou pro tuto službu.</span><span class="sxs-lookup"><span data-stu-id="504c1-146">With this command, Kubernetes creates a service and an [Azure load balancer rule](container-service-kubernetes-load-balancing.md) with a public IP address for the service.</span></span> 

<span data-ttu-id="504c1-147">Spuštěním následujícího příkazu zobrazte stav služby.</span><span class="sxs-lookup"><span data-stu-id="504c1-147">Run the following command to see the status of the service.</span></span>

```azurecli-interactive
kubectl get svc
```

<span data-ttu-id="504c1-148">Zpočátku se IP adresa bude zobrazovat jako `pending` (čekající).</span><span class="sxs-lookup"><span data-stu-id="504c1-148">Initially the IP address appears as `pending`.</span></span> <span data-ttu-id="504c1-149">Po několika minutách se externí IP adresa podu `iis` nastaví takto:</span><span class="sxs-lookup"><span data-stu-id="504c1-149">After a few minutes, the external IP address of the `iis` pod is set:</span></span>
  
```azurecli-interactive
NAME         CLUSTER-IP     EXTERNAL-IP     PORT(S)        AGE       
kubernetes   10.0.0.1       <none>          443/TCP        21h       
iis          10.0.111.25    13.64.158.233   80/TCP         22m
```

<span data-ttu-id="504c1-150">Pokud si chcete zobrazit úvodní stránku ISS na externí IP adrese, můžete použít libovolný webový prohlížeč:</span><span class="sxs-lookup"><span data-stu-id="504c1-150">You can use a web browser of your choice to see the default IIS welcome page at the external IP address:</span></span>

![Obrázek přechodu na službu IIS](./media/container-service-kubernetes-windows-walkthrough/kubernetes-iis.png)  


## <a name="delete-cluster"></a><span data-ttu-id="504c1-152">Odstranění clusteru</span><span class="sxs-lookup"><span data-stu-id="504c1-152">Delete cluster</span></span>
<span data-ttu-id="504c1-153">Pokud už cluster nepotřebujete, můžete k odebrání skupiny prostředků, služby kontejneru a všech souvisejících prostředků použít příkaz [az group delete](/cli/azure/group#delete).</span><span class="sxs-lookup"><span data-stu-id="504c1-153">When the cluster is no longer needed, you can use the [az group delete](/cli/azure/group#delete) command to remove the resource group, container service, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```


## <a name="next-steps"></a><span data-ttu-id="504c1-154">Další kroky</span><span class="sxs-lookup"><span data-stu-id="504c1-154">Next steps</span></span>

<span data-ttu-id="504c1-155">V tomto rychlém úvodním kurzu jste nasadili cluster Kubernetes, připojili se přes `kubectl` a nasadili pod s kontejnerem ISS.</span><span class="sxs-lookup"><span data-stu-id="504c1-155">In this quick start, you deployed a Kubernetes cluster, connected with `kubectl`, and deployed a pod with an IIS container.</span></span> <span data-ttu-id="504c1-156">Další informace o Azure Container Service získáte v kurzu o Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="504c1-156">To learn more about Azure Container Service, continue to the Kubernetes tutorial.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="504c1-157">Správa clusteru ACS Kubernetes</span><span class="sxs-lookup"><span data-stu-id="504c1-157">Manage an ACS Kubernetes cluster</span></span>](container-service-tutorial-kubernetes-prepare-app.md)
