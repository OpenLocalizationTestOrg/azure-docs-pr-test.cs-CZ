---
title: "kurz pro službu kontejneru aaaAzure – aktualizace aplikací | Microsoft Docs"
description: "Kurz pro Azure Container Service – aktualizace aplikace"
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
ms.date: 07/26/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: c467498bab7952926a18e45ffbb21051a98739d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="update-an-application-in-kubernetes"></a><span data-ttu-id="844d9-104">Aktualizace aplikace v Kubernetes</span><span class="sxs-lookup"><span data-stu-id="844d9-104">Update an application in Kubernetes</span></span>

<span data-ttu-id="844d9-105">Po nasazení aplikace v Kubernetes, můžete aktualizovat tak, že zadáte novou bitovou kopii kontejneru nebo verze bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="844d9-105">After you deploy an application in Kubernetes, it can be updated by specifying a new container image or image version.</span></span> <span data-ttu-id="844d9-106">Při aktualizaci aplikace zavedení aktualizace hello je vynášené, aby pouze část nasazení hello souběžně se aktualizuje.</span><span class="sxs-lookup"><span data-stu-id="844d9-106">When you update an application, hello update rollout is staged so that only a portion of hello deployment is concurrently updated.</span></span> <span data-ttu-id="844d9-107">Tato dvoufázové instalace aktualizace umožňuje tookeep aplikace hello během aktualizace hello a poskytuje mechanismus vrácení zpět, pokud dojde k selhání nasazení.</span><span class="sxs-lookup"><span data-stu-id="844d9-107">This staged update enables hello application tookeep running during hello update, and provides a rollback mechanism if a deployment failure occurs.</span></span> 

<span data-ttu-id="844d9-108">V tomto kurzu se aktualizuje půl součástí sedm, hello ukázkovou aplikaci Azure hlas.</span><span class="sxs-lookup"><span data-stu-id="844d9-108">In this tutorial, part six of seven, hello sample Azure Vote app is updated.</span></span> <span data-ttu-id="844d9-109">Úlohy, které zahrnují:</span><span class="sxs-lookup"><span data-stu-id="844d9-109">Tasks that you complete include:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="844d9-110">Aktualizace kód aplikace front-endu hello</span><span class="sxs-lookup"><span data-stu-id="844d9-110">Updating hello front-end application code</span></span>
> * <span data-ttu-id="844d9-111">Vytvoření bitové kopie aktualizované kontejneru</span><span class="sxs-lookup"><span data-stu-id="844d9-111">Creating an updated container image</span></span>
> * <span data-ttu-id="844d9-112">Vkládání hello kontejneru image tooAzure registru kontejneru</span><span class="sxs-lookup"><span data-stu-id="844d9-112">Pushing hello container image tooAzure Container Registry</span></span>
> * <span data-ttu-id="844d9-113">Nasazení bitové kopie hello aktualizované kontejneru</span><span class="sxs-lookup"><span data-stu-id="844d9-113">Deploying hello updated container image</span></span>

<span data-ttu-id="844d9-114">V následujících kurzech Operations Management Suite je nakonfigurované toomonitor hello Kubernetes clusteru.</span><span class="sxs-lookup"><span data-stu-id="844d9-114">In subsequent tutorials, Operations Management Suite is configured toomonitor hello Kubernetes cluster.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="844d9-115">Než začnete</span><span class="sxs-lookup"><span data-stu-id="844d9-115">Before you begin</span></span>

<span data-ttu-id="844d9-116">V předchozí kurzy byla aplikace zabalené do kontejneru image, hello image nahráli tooAzure registru kontejneru a cluster Kubernetes vytvořit.</span><span class="sxs-lookup"><span data-stu-id="844d9-116">In previous tutorials, an application was packaged into a container image, hello image uploaded tooAzure Container Registry, and a Kubernetes cluster created.</span></span> <span data-ttu-id="844d9-117">Hello aplikace pak byl na hello Kubernetes clusteru spusťte.</span><span class="sxs-lookup"><span data-stu-id="844d9-117">hello application was then run on hello Kubernetes cluster.</span></span> 

<span data-ttu-id="844d9-118">Pokud jste nedokončili tyto kroky a chcete toofollow společně, vrátí příliš[kurzu 1 – Vytvoření kontejneru image](./container-service-tutorial-kubernetes-prepare-app.md).</span><span class="sxs-lookup"><span data-stu-id="844d9-118">If you haven't completed these steps, and want toofollow along, return too[Tutorial 1 – Create container images](./container-service-tutorial-kubernetes-prepare-app.md).</span></span> 

## <a name="update-application"></a><span data-ttu-id="844d9-119">Aktualizace aplikace</span><span class="sxs-lookup"><span data-stu-id="844d9-119">Update application</span></span>

<span data-ttu-id="844d9-120">toocomplete hello kroky v tomto kurzu, budete musí mít klonovat kopii hello hlas Azure aplikace.</span><span class="sxs-lookup"><span data-stu-id="844d9-120">toocomplete hello steps in this tutorial, you must have cloned a copy of hello Azure Vote application.</span></span> <span data-ttu-id="844d9-121">V případě potřeby vytvořte tato klonovaný kopie s hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="844d9-121">If necessary, create this cloned copy with hello following command:</span></span>

```bash
git clone https://github.com/Azure-Samples/azure-voting-app-redis.git
```

<span data-ttu-id="844d9-122">Otevřete hello `config_file.cfg` soubor pomocí libovolného editoru kódu nebo text.</span><span class="sxs-lookup"><span data-stu-id="844d9-122">Open hello `config_file.cfg` file with any code or text editor.</span></span> <span data-ttu-id="844d9-123">Tento soubor v následujícím adresáři hello klonovat úložiště hello můžete najít.</span><span class="sxs-lookup"><span data-stu-id="844d9-123">You can find this file under hello following directory of hello cloned repo.</span></span>

```bash
 /azure-voting-app-redis/azure-vote/azure-vote/config_file.cfg
```

<span data-ttu-id="844d9-124">Změnit hodnoty hello `VOTE1VALUE` a `VOTE2VALUE`a pak hello soubor uložte.</span><span class="sxs-lookup"><span data-stu-id="844d9-124">Change hello values for `VOTE1VALUE` and `VOTE2VALUE`, and then save hello file.</span></span>

```bash
# UI Configurations
TITLE = 'Azure Voting App'
VOTE1VALUE = 'Blue'
VOTE2VALUE = 'Purple'
SHOWHOST = 'false'
```

<span data-ttu-id="844d9-125">Použití [docker compose](https://docs.docker.com/compose/) toore-vytvořit hello front-end bitové kopie a spuštění hello aktualizovat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="844d9-125">Use [docker-compose](https://docs.docker.com/compose/) toore-create hello front-end image and run hello updated application.</span></span>

```bash
docker-compose -f ./azure-voting-app-redis/docker-compose.yml up --build -d
```

## <a name="test-application-locally"></a><span data-ttu-id="844d9-126">Testovací aplikace místně</span><span class="sxs-lookup"><span data-stu-id="844d9-126">Test application locally</span></span>

<span data-ttu-id="844d9-127">Procházet příliš`http://localhost:8080` toosee hello aktualizovat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="844d9-127">Browse too`http://localhost:8080` toosee hello updated application.</span></span>

![Obrázek clusteru Kubernetes v Azure](media/container-service-kubernetes-tutorials/vote-app-updated.png)

## <a name="tag-and-push-images"></a><span data-ttu-id="844d9-129">Značky a nabízených bitové kopie</span><span class="sxs-lookup"><span data-stu-id="844d9-129">Tag and push images</span></span>

<span data-ttu-id="844d9-130">Značka hello *azure hlas front* bitovou kopii s loginServer hello hello kontejneru registru.</span><span class="sxs-lookup"><span data-stu-id="844d9-130">Tag hello *azure-vote-front* image with hello loginServer of hello container registry.</span></span>

<span data-ttu-id="844d9-131">Pokud používáte Azure kontejneru registru, získat název serveru hello přihlášení s hello [az acr seznamu](/cli/azure/acr#list) příkaz.</span><span class="sxs-lookup"><span data-stu-id="844d9-131">If you're using Azure Container Registry, get hello login server name with hello [az acr list](/cli/azure/acr#list) command.</span></span>

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

<span data-ttu-id="844d9-132">Použití [docker značka](https://docs.docker.com/engine/reference/commandline/tag/) tootag hello image.</span><span class="sxs-lookup"><span data-stu-id="844d9-132">Use [docker tag](https://docs.docker.com/engine/reference/commandline/tag/) tootag hello image.</span></span> <span data-ttu-id="844d9-133">Nahraďte `<acrLoginServer>` se název serveru registru kontejner Azure přihlášení nebo registru veřejný název hostitele.</span><span class="sxs-lookup"><span data-stu-id="844d9-133">Replace `<acrLoginServer>` with your Azure Container Registry login server name or public registry hostname.</span></span>

```bash
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:redis-v2
```

<span data-ttu-id="844d9-134">Použití [docker nabízené](https://docs.docker.com/engine/reference/commandline/push/) tooupload hello image tooyour registru.</span><span class="sxs-lookup"><span data-stu-id="844d9-134">Use [docker push](https://docs.docker.com/engine/reference/commandline/push/) tooupload hello image tooyour registry.</span></span> <span data-ttu-id="844d9-135">Nahraďte `<acrLoginServer>` se název serveru registru kontejner Azure přihlášení nebo registru veřejný název hostitele.</span><span class="sxs-lookup"><span data-stu-id="844d9-135">Replace `<acrLoginServer>` with your Azure Container Registry login server name or public registry hostname.</span></span>

```bash
docker push <acrLoginServer>/azure-vote-front:redis-v2
```

## <a name="deploy-update-application"></a><span data-ttu-id="844d9-136">Nasazení aktualizace aplikace</span><span class="sxs-lookup"><span data-stu-id="844d9-136">Deploy update application</span></span>

<span data-ttu-id="844d9-137">Maximální doba provozu tooensure, musí být spuštěna více instancí pod aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="844d9-137">tooensure maximum uptime, multiple instances of hello application pod must be running.</span></span> <span data-ttu-id="844d9-138">Tuto konfiguraci ověříte s hello [kubectl získat pod](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) příkaz.</span><span class="sxs-lookup"><span data-stu-id="844d9-138">Verify this configuration with hello [kubectl get pod](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) command.</span></span>

```bash
kubectl get pod
```

<span data-ttu-id="844d9-139">Výstup:</span><span class="sxs-lookup"><span data-stu-id="844d9-139">Output:</span></span>

```bash
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-217588096-5w632    1/1       Running   0          10m
azure-vote-front-233282510-b5pkz   1/1       Running   0          10m
azure-vote-front-233282510-dhrtr   1/1       Running   0          10m
azure-vote-front-233282510-pqbfk   1/1       Running   0          10m
```

<span data-ttu-id="844d9-140">Pokud nemáte k dispozici více pracovními stanicemi soustředěnými kolem systémem azure hlas front hello bitové kopie, škálovat hello *azure hlas front* nasazení.</span><span class="sxs-lookup"><span data-stu-id="844d9-140">If you don't have multiple pods running hello azure-vote-front image, scale hello *azure-vote-front* deployment.</span></span>


```azurecli-interactive
kubectl scale --replicas=3 deployment/azure-vote-front
```

<span data-ttu-id="844d9-141">tooupdate hello aplikace, použijte hello [kubectl sady](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#set) příkaz.</span><span class="sxs-lookup"><span data-stu-id="844d9-141">tooupdate hello application, use hello [kubectl set](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#set) command.</span></span> <span data-ttu-id="844d9-142">Aktualizace `<acrLoginServer>` s hello přihlášení serveru nebo název hostitele vašeho kontejneru registru.</span><span class="sxs-lookup"><span data-stu-id="844d9-142">Update `<acrLoginServer>` with hello login server or host name of your container registry.</span></span>

```azurecli-interactive
kubectl set image deployment azure-vote-front azure-vote-front=<acrLoginServer>/azure-vote-front:redis-v2
```

<span data-ttu-id="844d9-143">toomonitor hello nasazení, použijte hello [kubectl získat pod](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) příkaz.</span><span class="sxs-lookup"><span data-stu-id="844d9-143">toomonitor hello deployment, use hello [kubectl get pod](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) command.</span></span> <span data-ttu-id="844d9-144">Jako hello aktualizovat aplikace nasazena, jsou vaše pracovními stanicemi soustředěnými kolem ukončena a znovu vytvořit s hello novou bitovou kopii kontejneru.</span><span class="sxs-lookup"><span data-stu-id="844d9-144">As hello updated application is deployed, your pods are terminated and re-created with hello new container image.</span></span>

```azurecli-interactive
kubectl get pod
```

<span data-ttu-id="844d9-145">Výstup:</span><span class="sxs-lookup"><span data-stu-id="844d9-145">Output:</span></span>

```bash
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-2978095810-gq9g0   1/1       Running   0          5m
azure-vote-front-1297194256-tpjlg   1/1       Running   0         1m
azure-vote-front-1297194256-tptnx   1/1       Running   0         5m
azure-vote-front-1297194256-zktw9   1/1       Terminating   0         1m
```

## <a name="test-updated-application"></a><span data-ttu-id="844d9-146">Testovací aktualizovat aplikaci</span><span class="sxs-lookup"><span data-stu-id="844d9-146">Test updated application</span></span>

<span data-ttu-id="844d9-147">Získat hello externí IP adresu hello *azure hlas front* služby.</span><span class="sxs-lookup"><span data-stu-id="844d9-147">Get hello external IP address of hello *azure-vote-front* service.</span></span>

```azurecli-interactive
kubectl get service azure-vote-front
```

<span data-ttu-id="844d9-148">Procházet toohello IP adresu toosee hello aktualizovat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="844d9-148">Browse toohello IP address toosee hello updated application.</span></span>

![Obrázek clusteru Kubernetes v Azure](media/container-service-kubernetes-tutorials/vote-app-updated-external.png)

## <a name="next-steps"></a><span data-ttu-id="844d9-150">Další kroky</span><span class="sxs-lookup"><span data-stu-id="844d9-150">Next steps</span></span>

<span data-ttu-id="844d9-151">V tomto kurzu aktualizovat aplikaci a nasazuje clusteru Kubernetes tooa této aktualizace.</span><span class="sxs-lookup"><span data-stu-id="844d9-151">In this tutorial, you updated an application and rolled out this update tooa Kubernetes cluster.</span></span> <span data-ttu-id="844d9-152">byly dokončeny Hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="844d9-152">hello following tasks were completed:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="844d9-153">Kód aplikace front-endu aktualizované hello</span><span class="sxs-lookup"><span data-stu-id="844d9-153">Updated hello front-end application code</span></span>
> * <span data-ttu-id="844d9-154">Vytvořit bitovou kopii aktualizované kontejneru</span><span class="sxs-lookup"><span data-stu-id="844d9-154">Created an updated container image</span></span>
> * <span data-ttu-id="844d9-155">Poslat hello kontejneru image tooAzure registru kontejneru</span><span class="sxs-lookup"><span data-stu-id="844d9-155">Pushed hello container image tooAzure Container Registry</span></span>
> * <span data-ttu-id="844d9-156">Aplikace nasazené hello aktualizovat</span><span class="sxs-lookup"><span data-stu-id="844d9-156">Deployed hello updated application</span></span>

<span data-ttu-id="844d9-157">Posunutí další kurz toolearn toohello o toomonitor Kubernetes s Operations Management Suite.</span><span class="sxs-lookup"><span data-stu-id="844d9-157">Advance toohello next tutorial toolearn about how toomonitor Kubernetes with Operations Management Suite.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="844d9-158">Monitorování Kubernetes pomocí OMS</span><span class="sxs-lookup"><span data-stu-id="844d9-158">Monitor Kubernetes with OMS</span></span>](./container-service-tutorial-kubernetes-monitor.md)