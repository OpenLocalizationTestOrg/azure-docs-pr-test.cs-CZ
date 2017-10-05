---
title: "Kurz pro Azure Container Service - škálování aplikace | Microsoft Docs"
description: "Kurz pro Azure Container Service - škálování aplikace"
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: Docker, kontejnery, Micro-services, Kubernetes, Azure
ms.assetid: 
ms.service: container-service
ms.devlang: aurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: 62e70e34d06f5220734ff85c70a0c9b475f9579b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="scale-kubernetes-pods-and-kubernetes-infrastructure"></a><span data-ttu-id="90728-104">Škálování infrastruktury Kubernetes a pracovními stanicemi soustředěnými kolem Kubernetes</span><span class="sxs-lookup"><span data-stu-id="90728-104">Scale Kubernetes pods and Kubernetes infrastructure</span></span>

<span data-ttu-id="90728-105">Pokud jste byli po kurzů k, máte funkční Kubernetes cluster v Azure Container Service a nasazení aplikace Azure hlasování.</span><span class="sxs-lookup"><span data-stu-id="90728-105">If you've been following the tutorials, you have a working Kubernetes cluster in Azure Container Service and you deployed the Azure Voting app.</span></span> 

<span data-ttu-id="90728-106">V tomto kurzu, součástí pět 7, škálovat pracovními stanicemi soustředěnými kolem v aplikaci a zkuste to pod automatické škálování.</span><span class="sxs-lookup"><span data-stu-id="90728-106">In this tutorial, part five of seven, you scale out the pods in the app and try pod autoscaling.</span></span> <span data-ttu-id="90728-107">Můžete také zjistěte, jak škálování počtu uzlů agent virtuálního počítače Azure změnit kapacity clusteru pro hostování úloh.</span><span class="sxs-lookup"><span data-stu-id="90728-107">You also learn how to scale the number of Azure VM agent nodes to change the cluster's capacity for hosting workloads.</span></span> <span data-ttu-id="90728-108">Dokončené úkoly patří:</span><span class="sxs-lookup"><span data-stu-id="90728-108">Tasks completed include:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="90728-109">Ruční škálování pracovními stanicemi soustředěnými kolem Kubernetes</span><span class="sxs-lookup"><span data-stu-id="90728-109">Manually scaling Kubernetes pods</span></span>
> * <span data-ttu-id="90728-110">Konfigurace automatického škálování pracovními stanicemi soustředěnými kolem spuštění aplikace front-endu</span><span class="sxs-lookup"><span data-stu-id="90728-110">Configuring Autoscale pods running the app front end</span></span>
> * <span data-ttu-id="90728-111">Škálování uzlů agent Kubernetes Azure</span><span class="sxs-lookup"><span data-stu-id="90728-111">Scale the Kubernetes Azure agent nodes</span></span>

<span data-ttu-id="90728-112">V následujících kurzech hlas Azure bude aktualizována a Operations Management Suite konfigurované pro monitorování Kubernetes clusteru.</span><span class="sxs-lookup"><span data-stu-id="90728-112">In subsequent tutorials, the Azure Vote application is updated, and Operations Management Suite configured to monitor the Kubernetes cluster.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="90728-113">Než začnete</span><span class="sxs-lookup"><span data-stu-id="90728-113">Before you begin</span></span>

<span data-ttu-id="90728-114">V předchozích kurzy byla aplikace zabalené do kontejneru image, tento image nahrané do registru kontejner Azure a cluster Kubernetes vytvořit.</span><span class="sxs-lookup"><span data-stu-id="90728-114">In previous tutorials, an application was packaged into a container image, this image uploaded to Azure Container Registry, and a Kubernetes cluster created.</span></span> <span data-ttu-id="90728-115">Aplikace pak byl na Kubernetes clusteru spusťte.</span><span class="sxs-lookup"><span data-stu-id="90728-115">The application was then run on the Kubernetes cluster.</span></span> <span data-ttu-id="90728-116">Pokud se ještě provést tyto kroky a chcete sledovat, vrátit [kurzu 1 – Vytvoření kontejneru image](./container-service-tutorial-kubernetes-prepare-app.md).</span><span class="sxs-lookup"><span data-stu-id="90728-116">If you have not done these steps, and would like to follow along, return to the [Tutorial 1 – Create container images](./container-service-tutorial-kubernetes-prepare-app.md).</span></span> 

<span data-ttu-id="90728-117">Minimálně tento kurz vyžaduje Kubernetes clusteru s běžící aplikaci.</span><span class="sxs-lookup"><span data-stu-id="90728-117">At minimum, this tutorial requires a Kubernetes cluster with a running application.</span></span>

## <a name="manually-scale-pods"></a><span data-ttu-id="90728-118">Ručně škálovat, pracovními stanicemi soustředěnými kolem</span><span class="sxs-lookup"><span data-stu-id="90728-118">Manually scale pods</span></span>

<span data-ttu-id="90728-119">Proto úplně, Azure hlas front-endu a Redis instance byly nasazené, každý s jednu repliku.</span><span class="sxs-lookup"><span data-stu-id="90728-119">Thus far, the Azure Vote front-end and Redis instance have been deployed, each with a single replica.</span></span> <span data-ttu-id="90728-120">Pokud chcete ověřit, spusťte [kubectl získat](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) příkaz.</span><span class="sxs-lookup"><span data-stu-id="90728-120">To verify, run the [kubectl get](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) command.</span></span>

```azurecli-interactive
kubectl get pods
```

<span data-ttu-id="90728-121">Výstup:</span><span class="sxs-lookup"><span data-stu-id="90728-121">Output:</span></span>

```bash
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-2549686872-4d2r5   1/1       Running   0          31m
azure-vote-front-848767080-tf34m   1/1       Running   0          31m
```

<span data-ttu-id="90728-122">Ručně změnit počet pracovními stanicemi soustředěnými kolem v `azure-vote-front` nasazení pomocí [kubectl škálování](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#scale) příkaz.</span><span class="sxs-lookup"><span data-stu-id="90728-122">Manually change the number of pods in the `azure-vote-front` deployment using the [kubectl scale](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#scale) command.</span></span> <span data-ttu-id="90728-123">Tento příklad zvyšuje počet 5.</span><span class="sxs-lookup"><span data-stu-id="90728-123">This example increases the number to 5.</span></span>

```azurecli-interactive
kubectl scale --replicas=5 deployment/azure-vote-front
```

<span data-ttu-id="90728-124">Spustit [kubectl získat pracovními stanicemi soustředěnými kolem](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) k ověření, že Kubernetes vytváří pracovními stanicemi soustředěnými kolem.</span><span class="sxs-lookup"><span data-stu-id="90728-124">Run [kubectl get pods](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) to verify that Kubernetes is creating the pods.</span></span> <span data-ttu-id="90728-125">Za minutu další pracovními stanicemi soustředěnými kolem běží:</span><span class="sxs-lookup"><span data-stu-id="90728-125">After a minute or so, the additional pods are running:</span></span>

```azurecli-interactive
kubectl get pods
```

<span data-ttu-id="90728-126">Výstup:</span><span class="sxs-lookup"><span data-stu-id="90728-126">Output:</span></span>

```bash
NAME                                READY     STATUS    RESTARTS   AGE
azure-vote-back-2606967446-nmpcf    1/1       Running   0          15m
azure-vote-front-3309479140-2hfh0   1/1       Running   0          3m
azure-vote-front-3309479140-bzt05   1/1       Running   0          3m
azure-vote-front-3309479140-fvcvm   1/1       Running   0          3m
azure-vote-front-3309479140-hrbf2   1/1       Running   0          15m
azure-vote-front-3309479140-qphz8   1/1       Running   0          3m
```

## <a name="autoscale-pods"></a><span data-ttu-id="90728-127">Pracovními stanicemi soustředěnými kolem škálování</span><span class="sxs-lookup"><span data-stu-id="90728-127">Autoscale pods</span></span>

<span data-ttu-id="90728-128">Podporuje Kubernetes [automatické škálování vodorovné pod](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) upravit počet pracovními stanicemi soustředěnými kolem v nasazení v závislosti na využití procesoru nebo jiné vybrat metriky.</span><span class="sxs-lookup"><span data-stu-id="90728-128">Kubernetes supports [horizontal pod autoscaling](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) to adjust the number of pods in a deployment depending on CPU utilization or other select metrics.</span></span> 

<span data-ttu-id="90728-129">Pokud chcete použít autoscaler, musí mít vaše pracovními stanicemi soustředěnými kolem procesoru požadavky a omezení definovaná.</span><span class="sxs-lookup"><span data-stu-id="90728-129">To use the autoscaler, your pods must have CPU requests and limits defined.</span></span> <span data-ttu-id="90728-130">V `azure-vote-front` nasazení, front-end kontejneru požadavky 0,25 procesor limit 0,5 procesoru.</span><span class="sxs-lookup"><span data-stu-id="90728-130">In the `azure-vote-front` deployment, the front-end container requests 0.25 CPU, with a limit of 0.5 CPU.</span></span> <span data-ttu-id="90728-131">Nastavení vypadat podobně jako:</span><span class="sxs-lookup"><span data-stu-id="90728-131">The settings look like:</span></span>

```YAML
resources:
  requests:
     cpu: 250m
  limits:
     cpu: 500m
```

<span data-ttu-id="90728-132">Následující příklad používá [kubectl škálování](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#autoscale) příkaz pro škálování počtu pracovními stanicemi soustředěnými kolem v `azure-vote-front` nasazení.</span><span class="sxs-lookup"><span data-stu-id="90728-132">The following example uses the [kubectl autoscale](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#autoscale) command to autoscale the number of pods in the `azure-vote-front` deployment.</span></span> <span data-ttu-id="90728-133">Sem pokud využití procesoru přesahuje 50 %, autoscaler zvyšuje pracovními stanicemi soustředěnými kolem maximálně 10.</span><span class="sxs-lookup"><span data-stu-id="90728-133">Here, if CPU utilization exceeds 50%, the autoscaler increases the pods to a maximum of 10.</span></span>


```azurecli-interactive
kubectl autoscale deployment azure-vote-front --cpu-percent=50 --min=3 --max=10
```

<span data-ttu-id="90728-134">Pokud chcete zobrazit stav autoscaler, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="90728-134">To see the status of the autoscaler, run the following command:</span></span>

```azurecli-interactive
kubectl get hpa
```

<span data-ttu-id="90728-135">Výstup:</span><span class="sxs-lookup"><span data-stu-id="90728-135">Output:</span></span>

```bash
NAME               REFERENCE                     TARGETS    MINPODS   MAXPODS   REPLICAS   AGE
azure-vote-front   Deployment/azure-vote-front   0% / 50%   3         10        3          2m
```

<span data-ttu-id="90728-136">Po několika minutách, s minimálním zatížením hlas Azure aplikace a snižuje počet replik pod automaticky na 3.</span><span class="sxs-lookup"><span data-stu-id="90728-136">After a few minutes, with minimal load on the Azure Vote app, the number of pod replicas decreases automatically to 3.</span></span>

## <a name="scale-the-agents"></a><span data-ttu-id="90728-137">Škálování agentů</span><span class="sxs-lookup"><span data-stu-id="90728-137">Scale the agents</span></span>

<span data-ttu-id="90728-138">Pokud jste vytvořili Kubernetes clusteru pomocí výchozí příkazů v předchozí kurzu, má tři uzly agenta.</span><span class="sxs-lookup"><span data-stu-id="90728-138">If you created your Kubernetes cluster using default commands in the previous tutorial, it has three agent nodes.</span></span> <span data-ttu-id="90728-139">Počet agentů můžete upravit ručně, pokud máte v plánu více nebo méně kontejneru zatížení v clusteru.</span><span class="sxs-lookup"><span data-stu-id="90728-139">You can adjust the number of agents manually if you plan more or fewer container workloads on your cluster.</span></span> <span data-ttu-id="90728-140">Použití [škálování služby acs az](/cli/azure/acs#scale) příkaz, a zadat počet agentů v `--new-agent-count` parametr.</span><span class="sxs-lookup"><span data-stu-id="90728-140">Use the [az acs scale](/cli/azure/acs#scale) command, and specify the number of agents with the `--new-agent-count` parameter.</span></span>

<span data-ttu-id="90728-141">Následující příklad zvyšuje počet uzlů agent na 4 v Kubernetes clusteru s názvem *myK8sCluster*.</span><span class="sxs-lookup"><span data-stu-id="90728-141">The following example increases the number of agent nodes to 4 in the Kubernetes cluster named *myK8sCluster*.</span></span> <span data-ttu-id="90728-142">Příkaz trvá několik minut na dokončení.</span><span class="sxs-lookup"><span data-stu-id="90728-142">The command takes a couple of minutes to complete.</span></span>

```azurecli-interactive
az acs scale --resource-group=myResourceGroup --name=myK8SCluster --new-agent-count 4
```

<span data-ttu-id="90728-143">Výstup příkazu zobrazuje číslo agenta uzly v hodnotě `agentPoolProfiles:count`:</span><span class="sxs-lookup"><span data-stu-id="90728-143">The command output shows the number of agent nodes in the value of `agentPoolProfiles:count`:</span></span>

```azurecli
{
  "agentPoolProfiles": [
    {
      "count": 4,
      "dnsPrefix": "myK8SCluster-myK8SCluster-e44f25-k8s-agents",
      "fqdn": "",
      "name": "agentpools",
      "vmSize": "Standard_D2_v2"
    }
  ],
...

```

## <a name="next-steps"></a><span data-ttu-id="90728-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="90728-144">Next steps</span></span>

<span data-ttu-id="90728-145">V tomto kurzu použít v clusteru Kubernetes různé funkce škálování.</span><span class="sxs-lookup"><span data-stu-id="90728-145">In this tutorial, you used different scaling features in your Kubernetes cluster.</span></span> <span data-ttu-id="90728-146">Úlohy popsané součástí:</span><span class="sxs-lookup"><span data-stu-id="90728-146">Tasks covered included:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="90728-147">Ruční škálování pracovními stanicemi soustředěnými kolem Kubernetes</span><span class="sxs-lookup"><span data-stu-id="90728-147">Manually scaling Kubernetes pods</span></span>
> * <span data-ttu-id="90728-148">Konfigurace automatického škálování pracovními stanicemi soustředěnými kolem spuštění aplikace front-endu</span><span class="sxs-lookup"><span data-stu-id="90728-148">Configuring Autoscale pods running the app front end</span></span>
> * <span data-ttu-id="90728-149">Škálování uzlů agent Kubernetes Azure</span><span class="sxs-lookup"><span data-stu-id="90728-149">Scale the Kubernetes Azure agent nodes</span></span>

<span data-ttu-id="90728-150">Přechodu na v dalším kurzu se dozvíte o aktualizaci aplikace v Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="90728-150">Advance to the next tutorial to learn about updating application in Kubernetes.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="90728-151">Aktualizace aplikace v Kubernetes</span><span class="sxs-lookup"><span data-stu-id="90728-151">Update an application in Kubernetes</span></span>](./container-service-tutorial-kubernetes-app-update.md)

