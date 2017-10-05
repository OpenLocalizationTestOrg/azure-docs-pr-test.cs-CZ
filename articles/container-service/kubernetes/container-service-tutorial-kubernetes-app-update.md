---
title: "Kurz pro Azure Container Service – aktualizace aplikací | Microsoft Docs"
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
ms.openlocfilehash: db580da3e2d70892bc37305394df5be609ebb8a3
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="update-an-application-in-kubernetes"></a><span data-ttu-id="fad82-104">Aktualizace aplikace v Kubernetes</span><span class="sxs-lookup"><span data-stu-id="fad82-104">Update an application in Kubernetes</span></span>

<span data-ttu-id="fad82-105">Po nasazení aplikace v Kubernetes, můžete aktualizovat tak, že zadáte novou bitovou kopii kontejneru nebo verze bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="fad82-105">After you deploy an application in Kubernetes, it can be updated by specifying a new container image or image version.</span></span> <span data-ttu-id="fad82-106">Při aktualizaci aplikace je zavedení aktualizace připraveny tak, aby souběžně se aktualizuje pouze část nasazení.</span><span class="sxs-lookup"><span data-stu-id="fad82-106">When you update an application, the update rollout is staged so that only a portion of the deployment is concurrently updated.</span></span> <span data-ttu-id="fad82-107">Tato dvoufázové instalace aktualizace umožňuje aplikaci běžet během aktualizace a poskytuje mechanismus vrácení zpět, pokud dojde k selhání nasazení.</span><span class="sxs-lookup"><span data-stu-id="fad82-107">This staged update enables the application to keep running during the update, and provides a rollback mechanism if a deployment failure occurs.</span></span> 

<span data-ttu-id="fad82-108">V tomto kurzu, součástí půl 7, se aktualizuje ukázkovou aplikaci Azure hlas.</span><span class="sxs-lookup"><span data-stu-id="fad82-108">In this tutorial, part six of seven, the sample Azure Vote app is updated.</span></span> <span data-ttu-id="fad82-109">Úlohy, které zahrnují:</span><span class="sxs-lookup"><span data-stu-id="fad82-109">Tasks that you complete include:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="fad82-110">Aktualizace kód aplikace front-endu</span><span class="sxs-lookup"><span data-stu-id="fad82-110">Updating the front-end application code</span></span>
> * <span data-ttu-id="fad82-111">Vytvoření bitové kopie aktualizované kontejneru</span><span class="sxs-lookup"><span data-stu-id="fad82-111">Creating an updated container image</span></span>
> * <span data-ttu-id="fad82-112">Když zavedete bitovou kopii kontejneru registru kontejner Azure</span><span class="sxs-lookup"><span data-stu-id="fad82-112">Pushing the container image to Azure Container Registry</span></span>
> * <span data-ttu-id="fad82-113">Nasazení bitové kopie aktualizované kontejneru</span><span class="sxs-lookup"><span data-stu-id="fad82-113">Deploying the updated container image</span></span>

<span data-ttu-id="fad82-114">V následujících kurzech Operations Management Suite nakonfigurován ke sledování Kubernetes clusteru.</span><span class="sxs-lookup"><span data-stu-id="fad82-114">In subsequent tutorials, Operations Management Suite is configured to monitor the Kubernetes cluster.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="fad82-115">Než začnete</span><span class="sxs-lookup"><span data-stu-id="fad82-115">Before you begin</span></span>

<span data-ttu-id="fad82-116">V předchozích kurzech byla aplikace zabalené do kontejneru image, image nahrané do registru kontejner Azure a cluster Kubernetes vytvořili.</span><span class="sxs-lookup"><span data-stu-id="fad82-116">In previous tutorials, an application was packaged into a container image, the image uploaded to Azure Container Registry, and a Kubernetes cluster created.</span></span> <span data-ttu-id="fad82-117">Aplikace pak byl na Kubernetes clusteru spusťte.</span><span class="sxs-lookup"><span data-stu-id="fad82-117">The application was then run on the Kubernetes cluster.</span></span> 

<span data-ttu-id="fad82-118">Pokud jste nedokončili tyto kroky a chcete sledovat, vrátit [kurzu 1 – Vytvoření kontejneru image](./container-service-tutorial-kubernetes-prepare-app.md).</span><span class="sxs-lookup"><span data-stu-id="fad82-118">If you haven't completed these steps, and want to follow along, return to [Tutorial 1 – Create container images](./container-service-tutorial-kubernetes-prepare-app.md).</span></span> 

## <a name="update-application"></a><span data-ttu-id="fad82-119">Aktualizace aplikace</span><span class="sxs-lookup"><span data-stu-id="fad82-119">Update application</span></span>

<span data-ttu-id="fad82-120">Pokud chcete provést kroky v tomto kurzu, musí mít klonovat kopie aplikace Azure hlas.</span><span class="sxs-lookup"><span data-stu-id="fad82-120">To complete the steps in this tutorial, you must have cloned a copy of the Azure Vote application.</span></span> <span data-ttu-id="fad82-121">V případě potřeby vytvořte tato klonovaný kopie pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="fad82-121">If necessary, create this cloned copy with the following command:</span></span>

```bash
git clone https://github.com/Azure-Samples/azure-voting-app-redis.git
```

<span data-ttu-id="fad82-122">Otevřete `config_file.cfg` soubor pomocí libovolného editoru kódu nebo text.</span><span class="sxs-lookup"><span data-stu-id="fad82-122">Open the `config_file.cfg` file with any code or text editor.</span></span> <span data-ttu-id="fad82-123">Můžete najít tento soubor v následujícím adresáři klonovaný úložišti.</span><span class="sxs-lookup"><span data-stu-id="fad82-123">You can find this file under the following directory of the cloned repo.</span></span>

```bash
 /azure-voting-app-redis/azure-vote/azure-vote/config_file.cfg
```

<span data-ttu-id="fad82-124">Změnit hodnoty nastavení `VOTE1VALUE` a `VOTE2VALUE`a pak soubor uložte.</span><span class="sxs-lookup"><span data-stu-id="fad82-124">Change the values for `VOTE1VALUE` and `VOTE2VALUE`, and then save the file.</span></span>

```bash
# UI Configurations
TITLE = 'Azure Voting App'
VOTE1VALUE = 'Blue'
VOTE2VALUE = 'Purple'
SHOWHOST = 'false'
```

<span data-ttu-id="fad82-125">Použití [docker compose](https://docs.docker.com/compose/) znovu vytvořit bitovou kopii front-endu a spustit aktualizovanou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="fad82-125">Use [docker-compose](https://docs.docker.com/compose/) to re-create the front-end image and run the updated application.</span></span>

```bash
docker-compose -f ./azure-voting-app-redis/docker-compose.yml up --build -d
```

## <a name="test-application-locally"></a><span data-ttu-id="fad82-126">Testovací aplikace místně</span><span class="sxs-lookup"><span data-stu-id="fad82-126">Test application locally</span></span>

<span data-ttu-id="fad82-127">Přejděte do `http://localhost:8080` zobrazíte aktualizovanou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="fad82-127">Browse to `http://localhost:8080` to see the updated application.</span></span>

![Obrázek clusteru Kubernetes v Azure](media/container-service-kubernetes-tutorials/vote-app-updated.png)

## <a name="tag-and-push-images"></a><span data-ttu-id="fad82-129">Značky a nabízených bitové kopie</span><span class="sxs-lookup"><span data-stu-id="fad82-129">Tag and push images</span></span>

<span data-ttu-id="fad82-130">Značka *azure hlas front* bitovou kopii s loginServer registru kontejneru.</span><span class="sxs-lookup"><span data-stu-id="fad82-130">Tag the *azure-vote-front* image with the loginServer of the container registry.</span></span>

<span data-ttu-id="fad82-131">Pokud používáte Azure kontejneru registru, získat přihlašovací jméno pro server s [az acr seznamu](/cli/azure/acr#list) příkaz.</span><span class="sxs-lookup"><span data-stu-id="fad82-131">If you're using Azure Container Registry, get the login server name with the [az acr list](/cli/azure/acr#list) command.</span></span>

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

<span data-ttu-id="fad82-132">Použití [docker značky](https://docs.docker.com/engine/reference/commandline/tag/) k označení bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="fad82-132">Use [docker tag](https://docs.docker.com/engine/reference/commandline/tag/) to tag the image.</span></span> <span data-ttu-id="fad82-133">Nahraďte `<acrLoginServer>` se název serveru registru kontejner Azure přihlášení nebo registru veřejný název hostitele.</span><span class="sxs-lookup"><span data-stu-id="fad82-133">Replace `<acrLoginServer>` with your Azure Container Registry login server name or public registry hostname.</span></span>

```bash
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:redis-v2
```

<span data-ttu-id="fad82-134">Použití [docker nabízené](https://docs.docker.com/engine/reference/commandline/push/) Odeslat bitovou kopii do registru.</span><span class="sxs-lookup"><span data-stu-id="fad82-134">Use [docker push](https://docs.docker.com/engine/reference/commandline/push/) to upload the image to your registry.</span></span> <span data-ttu-id="fad82-135">Nahraďte `<acrLoginServer>` se název serveru registru kontejner Azure přihlášení nebo registru veřejný název hostitele.</span><span class="sxs-lookup"><span data-stu-id="fad82-135">Replace `<acrLoginServer>` with your Azure Container Registry login server name or public registry hostname.</span></span>

```bash
docker push <acrLoginServer>/azure-vote-front:redis-v2
```

## <a name="deploy-update-application"></a><span data-ttu-id="fad82-136">Nasazení aktualizace aplikace</span><span class="sxs-lookup"><span data-stu-id="fad82-136">Deploy update application</span></span>

<span data-ttu-id="fad82-137">Aby maximální doba provozu, musí být spuštěna více instancí pod aplikace.</span><span class="sxs-lookup"><span data-stu-id="fad82-137">To ensure maximum uptime, multiple instances of the application pod must be running.</span></span> <span data-ttu-id="fad82-138">Ověřte, tato konfigurace se [kubectl získat pod](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) příkaz.</span><span class="sxs-lookup"><span data-stu-id="fad82-138">Verify this configuration with the [kubectl get pod](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) command.</span></span>

```bash
kubectl get pod
```

<span data-ttu-id="fad82-139">Výstup:</span><span class="sxs-lookup"><span data-stu-id="fad82-139">Output:</span></span>

```bash
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-217588096-5w632    1/1       Running   0          10m
azure-vote-front-233282510-b5pkz   1/1       Running   0          10m
azure-vote-front-233282510-dhrtr   1/1       Running   0          10m
azure-vote-front-233282510-pqbfk   1/1       Running   0          10m
```

<span data-ttu-id="fad82-140">Pokud nemáte k dispozici více pracovními stanicemi soustředěnými kolem systémem bitovou kopii azure hlas front, škálování *azure hlas front* nasazení.</span><span class="sxs-lookup"><span data-stu-id="fad82-140">If you don't have multiple pods running the azure-vote-front image, scale the *azure-vote-front* deployment.</span></span>


```azurecli-interactive
kubectl scale --replicas=3 deployment/azure-vote-front
```

<span data-ttu-id="fad82-141">Chcete-li aktualizovat aplikace, použijte [kubectl sady](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#set) příkaz.</span><span class="sxs-lookup"><span data-stu-id="fad82-141">To update the application, use the [kubectl set](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#set) command.</span></span> <span data-ttu-id="fad82-142">Aktualizace `<acrLoginServer>` s přihlášení serveru nebo název hostitele vašeho kontejneru registru.</span><span class="sxs-lookup"><span data-stu-id="fad82-142">Update `<acrLoginServer>` with the login server or host name of your container registry.</span></span>

```azurecli-interactive
kubectl set image deployment azure-vote-front azure-vote-front=<acrLoginServer>/azure-vote-front:redis-v2
```

<span data-ttu-id="fad82-143">Ke sledování nasazení, použijte [kubectl získat pod](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) příkaz.</span><span class="sxs-lookup"><span data-stu-id="fad82-143">To monitor the deployment, use the [kubectl get pod](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) command.</span></span> <span data-ttu-id="fad82-144">Jako aktualizované aplikace nasazena, jsou vaše pracovními stanicemi soustředěnými kolem ukončena a znovu vytvořit s novou bitovou kopii kontejneru.</span><span class="sxs-lookup"><span data-stu-id="fad82-144">As the updated application is deployed, your pods are terminated and re-created with the new container image.</span></span>

```azurecli-interactive
kubectl get pod
```

<span data-ttu-id="fad82-145">Výstup:</span><span class="sxs-lookup"><span data-stu-id="fad82-145">Output:</span></span>

```bash
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-2978095810-gq9g0   1/1       Running   0          5m
azure-vote-front-1297194256-tpjlg   1/1       Running   0         1m
azure-vote-front-1297194256-tptnx   1/1       Running   0         5m
azure-vote-front-1297194256-zktw9   1/1       Terminating   0         1m
```

## <a name="test-updated-application"></a><span data-ttu-id="fad82-146">Testovací aktualizovat aplikaci</span><span class="sxs-lookup"><span data-stu-id="fad82-146">Test updated application</span></span>

<span data-ttu-id="fad82-147">Získat externí IP adresu *azure hlas front* služby.</span><span class="sxs-lookup"><span data-stu-id="fad82-147">Get the external IP address of the *azure-vote-front* service.</span></span>

```azurecli-interactive
kubectl get service azure-vote-front
```

<span data-ttu-id="fad82-148">Přejděte na IP adresu, kterou najdete v části aktualizovanou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="fad82-148">Browse to the IP address to see the updated application.</span></span>

![Obrázek clusteru Kubernetes v Azure](media/container-service-kubernetes-tutorials/vote-app-updated-external.png)

## <a name="next-steps"></a><span data-ttu-id="fad82-150">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fad82-150">Next steps</span></span>

<span data-ttu-id="fad82-151">V tomto kurzu aktualizovat aplikaci a vrátit se tato aktualizace na clusteru s podporou Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="fad82-151">In this tutorial, you updated an application and rolled out this update to a Kubernetes cluster.</span></span> <span data-ttu-id="fad82-152">Byly dokončit následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="fad82-152">The following tasks were completed:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="fad82-153">Aktualizovat kód aplikace front-endu</span><span class="sxs-lookup"><span data-stu-id="fad82-153">Updated the front-end application code</span></span>
> * <span data-ttu-id="fad82-154">Vytvořit bitovou kopii aktualizované kontejneru</span><span class="sxs-lookup"><span data-stu-id="fad82-154">Created an updated container image</span></span>
> * <span data-ttu-id="fad82-155">Nainstaluje bitovou kopii kontejneru do registru kontejner Azure</span><span class="sxs-lookup"><span data-stu-id="fad82-155">Pushed the container image to Azure Container Registry</span></span>
> * <span data-ttu-id="fad82-156">Aktualizovaná aplikace nasazena</span><span class="sxs-lookup"><span data-stu-id="fad82-156">Deployed the updated application</span></span>

<span data-ttu-id="fad82-157">Přechodu na v dalším kurzu se dozvíte o tom, jak sledovat Kubernetes s Operations Management Suite.</span><span class="sxs-lookup"><span data-stu-id="fad82-157">Advance to the next tutorial to learn about how to monitor Kubernetes with Operations Management Suite.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="fad82-158">Monitorování Kubernetes pomocí OMS</span><span class="sxs-lookup"><span data-stu-id="fad82-158">Monitor Kubernetes with OMS</span></span>](./container-service-tutorial-kubernetes-monitor.md)