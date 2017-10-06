---
title: "kurz pro službu kontejneru aaaAzure – škálování aplikace | Microsoft Docs"
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
ms.openlocfilehash: 29571eef0fd91bd6b40f00d8c018539f320179bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="scale-kubernetes-pods-and-kubernetes-infrastructure"></a><span data-ttu-id="5b928-104">Škálování infrastruktury Kubernetes a pracovními stanicemi soustředěnými kolem Kubernetes</span><span class="sxs-lookup"><span data-stu-id="5b928-104">Scale Kubernetes pods and Kubernetes infrastructure</span></span>

<span data-ttu-id="5b928-105">Pokud jste byli po hello kurzy, máte funkční Kubernetes cluster v Azure Container Service a nasazení aplikace Azure hlasování hello.</span><span class="sxs-lookup"><span data-stu-id="5b928-105">If you've been following hello tutorials, you have a working Kubernetes cluster in Azure Container Service and you deployed hello Azure Voting app.</span></span> 

<span data-ttu-id="5b928-106">V tomto kurzu, součástí pět 7, škálovat hello pracovními stanicemi soustředěnými kolem v hello aplikaci a akci pod automatické škálování.</span><span class="sxs-lookup"><span data-stu-id="5b928-106">In this tutorial, part five of seven, you scale out hello pods in hello app and try pod autoscaling.</span></span> <span data-ttu-id="5b928-107">Také zjistíte, jak tooscale hello počet toochange uzly agenta virtuálního počítače Azure hello kapacity na clusteru pro hostování úloh.</span><span class="sxs-lookup"><span data-stu-id="5b928-107">You also learn how tooscale hello number of Azure VM agent nodes toochange hello cluster's capacity for hosting workloads.</span></span> <span data-ttu-id="5b928-108">Dokončené úkoly patří:</span><span class="sxs-lookup"><span data-stu-id="5b928-108">Tasks completed include:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="5b928-109">Ruční škálování pracovními stanicemi soustředěnými kolem Kubernetes</span><span class="sxs-lookup"><span data-stu-id="5b928-109">Manually scaling Kubernetes pods</span></span>
> * <span data-ttu-id="5b928-110">Konfigurace automatického škálování pracovními stanicemi soustředěnými kolem systémem hello aplikace front-endu</span><span class="sxs-lookup"><span data-stu-id="5b928-110">Configuring Autoscale pods running hello app front end</span></span>
> * <span data-ttu-id="5b928-111">Škálování hello Kubernetes Azure agent uzly</span><span class="sxs-lookup"><span data-stu-id="5b928-111">Scale hello Kubernetes Azure agent nodes</span></span>

<span data-ttu-id="5b928-112">V následujících kurzech se aktualizuje hello hlas Azure aplikace a služby Operations Management Suite nakonfigurovaný toomonitor hello Kubernetes clusteru.</span><span class="sxs-lookup"><span data-stu-id="5b928-112">In subsequent tutorials, hello Azure Vote application is updated, and Operations Management Suite configured toomonitor hello Kubernetes cluster.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="5b928-113">Než začnete</span><span class="sxs-lookup"><span data-stu-id="5b928-113">Before you begin</span></span>

<span data-ttu-id="5b928-114">V předchozí kurzy aplikace se zabalí do kontejneru image, tuto bitovou kopii nahráli tooAzure registru kontejneru a cluster Kubernetes vytvořit.</span><span class="sxs-lookup"><span data-stu-id="5b928-114">In previous tutorials, an application was packaged into a container image, this image uploaded tooAzure Container Registry, and a Kubernetes cluster created.</span></span> <span data-ttu-id="5b928-115">Hello aplikace pak byl na hello Kubernetes clusteru spusťte.</span><span class="sxs-lookup"><span data-stu-id="5b928-115">hello application was then run on hello Kubernetes cluster.</span></span> <span data-ttu-id="5b928-116">Pokud jste ještě provést tyto kroky a chcete toofollow společně, vrátí toohello [kurzu 1 – Vytvoření kontejneru image](./container-service-tutorial-kubernetes-prepare-app.md).</span><span class="sxs-lookup"><span data-stu-id="5b928-116">If you have not done these steps, and would like toofollow along, return toohello [Tutorial 1 – Create container images](./container-service-tutorial-kubernetes-prepare-app.md).</span></span> 

<span data-ttu-id="5b928-117">Minimálně tento kurz vyžaduje Kubernetes clusteru s běžící aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5b928-117">At minimum, this tutorial requires a Kubernetes cluster with a running application.</span></span>

## <a name="manually-scale-pods"></a><span data-ttu-id="5b928-118">Ručně škálovat, pracovními stanicemi soustředěnými kolem</span><span class="sxs-lookup"><span data-stu-id="5b928-118">Manually scale pods</span></span>

<span data-ttu-id="5b928-119">Proto daleko hello Azure hlas front-endu a Redis instance nainstalovaná, každý s jednu repliku.</span><span class="sxs-lookup"><span data-stu-id="5b928-119">Thus far, hello Azure Vote front-end and Redis instance have been deployed, each with a single replica.</span></span> <span data-ttu-id="5b928-120">tooverify, spusťte hello [kubectl získat](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) příkaz.</span><span class="sxs-lookup"><span data-stu-id="5b928-120">tooverify, run hello [kubectl get](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) command.</span></span>

```azurecli-interactive
kubectl get pods
```

<span data-ttu-id="5b928-121">Výstup:</span><span class="sxs-lookup"><span data-stu-id="5b928-121">Output:</span></span>

```bash
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-2549686872-4d2r5   1/1       Running   0          31m
azure-vote-front-848767080-tf34m   1/1       Running   0          31m
```

<span data-ttu-id="5b928-122">Ruční změna hello počet pracovními stanicemi soustředěnými kolem v hello `azure-vote-front` nasazení pomocí hello [kubectl škálování](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#scale) příkaz.</span><span class="sxs-lookup"><span data-stu-id="5b928-122">Manually change hello number of pods in hello `azure-vote-front` deployment using hello [kubectl scale](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#scale) command.</span></span> <span data-ttu-id="5b928-123">Tento příklad zvyšuje počet too5 hello.</span><span class="sxs-lookup"><span data-stu-id="5b928-123">This example increases hello number too5.</span></span>

```azurecli-interactive
kubectl scale --replicas=5 deployment/azure-vote-front
```

<span data-ttu-id="5b928-124">Spustit [kubectl získat pracovními stanicemi soustředěnými kolem](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) tooverify, Kubernetes vytváří pracovními stanicemi soustředěnými kolem hello.</span><span class="sxs-lookup"><span data-stu-id="5b928-124">Run [kubectl get pods](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) tooverify that Kubernetes is creating hello pods.</span></span> <span data-ttu-id="5b928-125">Za minutu jsou spuštěny další pracovními stanicemi soustředěnými kolem hello:</span><span class="sxs-lookup"><span data-stu-id="5b928-125">After a minute or so, hello additional pods are running:</span></span>

```azurecli-interactive
kubectl get pods
```

<span data-ttu-id="5b928-126">Výstup:</span><span class="sxs-lookup"><span data-stu-id="5b928-126">Output:</span></span>

```bash
NAME                                READY     STATUS    RESTARTS   AGE
azure-vote-back-2606967446-nmpcf    1/1       Running   0          15m
azure-vote-front-3309479140-2hfh0   1/1       Running   0          3m
azure-vote-front-3309479140-bzt05   1/1       Running   0          3m
azure-vote-front-3309479140-fvcvm   1/1       Running   0          3m
azure-vote-front-3309479140-hrbf2   1/1       Running   0          15m
azure-vote-front-3309479140-qphz8   1/1       Running   0          3m
```

## <a name="autoscale-pods"></a><span data-ttu-id="5b928-127">Pracovními stanicemi soustředěnými kolem škálování</span><span class="sxs-lookup"><span data-stu-id="5b928-127">Autoscale pods</span></span>

<span data-ttu-id="5b928-128">Podporuje Kubernetes [automatické škálování vodorovné pod](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) tooadjust hello počet pracovními stanicemi soustředěnými kolem v nasazení v závislosti na využití procesoru nebo jiné vybrat metriky.</span><span class="sxs-lookup"><span data-stu-id="5b928-128">Kubernetes supports [horizontal pod autoscaling](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) tooadjust hello number of pods in a deployment depending on CPU utilization or other select metrics.</span></span> 

<span data-ttu-id="5b928-129">toouse hello autoscaler, vaše pracovními stanicemi soustředěnými kolem musí mít procesor požadavky a omezení definovaná.</span><span class="sxs-lookup"><span data-stu-id="5b928-129">toouse hello autoscaler, your pods must have CPU requests and limits defined.</span></span> <span data-ttu-id="5b928-130">V hello `azure-vote-front` nasazení hello front-end kontejneru požadavky 0,25 procesor limit 0,5 procesoru.</span><span class="sxs-lookup"><span data-stu-id="5b928-130">In hello `azure-vote-front` deployment, hello front-end container requests 0.25 CPU, with a limit of 0.5 CPU.</span></span> <span data-ttu-id="5b928-131">nastavení Hello vypadat podobně jako:</span><span class="sxs-lookup"><span data-stu-id="5b928-131">hello settings look like:</span></span>

```YAML
resources:
  requests:
     cpu: 250m
  limits:
     cpu: 500m
```

<span data-ttu-id="5b928-132">Hello následující příklad používá hello [kubectl škálování](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#autoscale) příkaz tooautoscale hello počet pracovními stanicemi soustředěnými kolem v hello `azure-vote-front` nasazení.</span><span class="sxs-lookup"><span data-stu-id="5b928-132">hello following example uses hello [kubectl autoscale](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#autoscale) command tooautoscale hello number of pods in hello `azure-vote-front` deployment.</span></span> <span data-ttu-id="5b928-133">Sem pokud využití procesoru přesahuje 50 %, hello autoscaler zvyšuje hello pracovními stanicemi soustředěnými kolem tooa maximálně 10.</span><span class="sxs-lookup"><span data-stu-id="5b928-133">Here, if CPU utilization exceeds 50%, hello autoscaler increases hello pods tooa maximum of 10.</span></span>


```azurecli-interactive
kubectl autoscale deployment azure-vote-front --cpu-percent=50 --min=3 --max=10
```

<span data-ttu-id="5b928-134">Stav hello toosee hello autoscaler, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="5b928-134">toosee hello status of hello autoscaler, run hello following command:</span></span>

```azurecli-interactive
kubectl get hpa
```

<span data-ttu-id="5b928-135">Výstup:</span><span class="sxs-lookup"><span data-stu-id="5b928-135">Output:</span></span>

```bash
NAME               REFERENCE                     TARGETS    MINPODS   MAXPODS   REPLICAS   AGE
azure-vote-front   Deployment/azure-vote-front   0% / 50%   3         10        3          2m
```

<span data-ttu-id="5b928-136">Po několika minutách, s minimálním zatížením hello aplikace Azure hlas snižuje hello počet replik pod automaticky too3.</span><span class="sxs-lookup"><span data-stu-id="5b928-136">After a few minutes, with minimal load on hello Azure Vote app, hello number of pod replicas decreases automatically too3.</span></span>

## <a name="scale-hello-agents"></a><span data-ttu-id="5b928-137">Škálování hello agentů</span><span class="sxs-lookup"><span data-stu-id="5b928-137">Scale hello agents</span></span>

<span data-ttu-id="5b928-138">Pokud jste vytvořili pomocí výchozí příkazů v předchozí kurzu hello Kubernetes clusteru, má tři uzly agenta.</span><span class="sxs-lookup"><span data-stu-id="5b928-138">If you created your Kubernetes cluster using default commands in hello previous tutorial, it has three agent nodes.</span></span> <span data-ttu-id="5b928-139">Hello počtu agentů, můžete upravit ručně, pokud máte v plánu více nebo méně kontejneru zatížení v clusteru.</span><span class="sxs-lookup"><span data-stu-id="5b928-139">You can adjust hello number of agents manually if you plan more or fewer container workloads on your cluster.</span></span> <span data-ttu-id="5b928-140">Použití hello [škálování služby acs az](/cli/azure/acs#scale) příkaz, a zadejte číslo hello agentů s hello `--new-agent-count` parametr.</span><span class="sxs-lookup"><span data-stu-id="5b928-140">Use hello [az acs scale](/cli/azure/acs#scale) command, and specify hello number of agents with hello `--new-agent-count` parameter.</span></span>

<span data-ttu-id="5b928-141">Hello následující příklad zvyšuje hello počet agenta too4 uzly v clusteru Kubernetes hello s názvem *myK8sCluster*.</span><span class="sxs-lookup"><span data-stu-id="5b928-141">hello following example increases hello number of agent nodes too4 in hello Kubernetes cluster named *myK8sCluster*.</span></span> <span data-ttu-id="5b928-142">příkaz Hello bude trvat několik minut toocomplete.</span><span class="sxs-lookup"><span data-stu-id="5b928-142">hello command takes a couple of minutes toocomplete.</span></span>

```azurecli-interactive
az acs scale --resource-group=myResourceGroup --name=myK8SCluster --new-agent-count 4
```

<span data-ttu-id="5b928-143">Hello výstupu příkazu se zobrazí hello číslo agenta uzly v hello hodnotu `agentPoolProfiles:count`:</span><span class="sxs-lookup"><span data-stu-id="5b928-143">hello command output shows hello number of agent nodes in hello value of `agentPoolProfiles:count`:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="5b928-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5b928-144">Next steps</span></span>

<span data-ttu-id="5b928-145">V tomto kurzu použít v clusteru Kubernetes různé funkce škálování.</span><span class="sxs-lookup"><span data-stu-id="5b928-145">In this tutorial, you used different scaling features in your Kubernetes cluster.</span></span> <span data-ttu-id="5b928-146">Úlohy popsané součástí:</span><span class="sxs-lookup"><span data-stu-id="5b928-146">Tasks covered included:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="5b928-147">Ruční škálování pracovními stanicemi soustředěnými kolem Kubernetes</span><span class="sxs-lookup"><span data-stu-id="5b928-147">Manually scaling Kubernetes pods</span></span>
> * <span data-ttu-id="5b928-148">Konfigurace automatického škálování pracovními stanicemi soustředěnými kolem systémem hello aplikace front-endu</span><span class="sxs-lookup"><span data-stu-id="5b928-148">Configuring Autoscale pods running hello app front end</span></span>
> * <span data-ttu-id="5b928-149">Škálování hello Kubernetes Azure agent uzly</span><span class="sxs-lookup"><span data-stu-id="5b928-149">Scale hello Kubernetes Azure agent nodes</span></span>

<span data-ttu-id="5b928-150">Další kurz toolearn toohello o aktualizaci aplikace v Kubernetes zálohy.</span><span class="sxs-lookup"><span data-stu-id="5b928-150">Advance toohello next tutorial toolearn about updating application in Kubernetes.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="5b928-151">Aktualizace aplikace v Kubernetes</span><span class="sxs-lookup"><span data-stu-id="5b928-151">Update an application in Kubernetes</span></span>](./container-service-tutorial-kubernetes-app-update.md)

