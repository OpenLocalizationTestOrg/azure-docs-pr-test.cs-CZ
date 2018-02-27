---
title: "Kurz Azure Container Service – Aktualizace aplikace"
description: "Kurz Azure Container Service – Aktualizace aplikace"
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 09/14/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 5ecaa3a79270e29ac002e91065f7df4f7e8914e7
ms.sourcegitcommit: 694e40a193980dea1e2f945471071f11030d5641
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
# <a name="update-an-application-in-kubernetes"></a><span data-ttu-id="dd151-103">Aktualizace aplikace v Kubernetes</span><span class="sxs-lookup"><span data-stu-id="dd151-103">Update an application in Kubernetes</span></span>

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

<span data-ttu-id="dd151-104">Aplikaci je možné po nasazení v Kubernetes aktualizovat zadáním nové image kontejneru nebo verze image.</span><span class="sxs-lookup"><span data-stu-id="dd151-104">After an application has been deployed in Kubernetes, it can be updated by specifying a new container image or image version.</span></span> <span data-ttu-id="dd151-105">Aktualizace je přitom fázovaná, takže se současně aktualizuje jenom část nasazení.</span><span class="sxs-lookup"><span data-stu-id="dd151-105">When doing so, the update is staged so that only a portion of the deployment is concurrently updated.</span></span> <span data-ttu-id="dd151-106">Tato fázovaná aktualizace umožňuje, aby aplikace během aktualizace běžela.</span><span class="sxs-lookup"><span data-stu-id="dd151-106">This staged update enables the application to keep running during the update.</span></span> <span data-ttu-id="dd151-107">Poskytuje také mechanismus vrácení zpět pro případ, že dojde k selhání nasazení.</span><span class="sxs-lookup"><span data-stu-id="dd151-107">It also provides a rollback mechanism if a deployment failure occurs.</span></span> 

<span data-ttu-id="dd151-108">V tomto kurzu, který je šestou částí sedmidílné série, se aktualizuje aplikace Azure Vote.</span><span class="sxs-lookup"><span data-stu-id="dd151-108">In this tutorial, part six of seven, the sample Azure Vote app is updated.</span></span> <span data-ttu-id="dd151-109">Úlohy, které provedete:</span><span class="sxs-lookup"><span data-stu-id="dd151-109">Tasks that you complete include:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="dd151-110">Aktualizace aplikačního kódu front-endu</span><span class="sxs-lookup"><span data-stu-id="dd151-110">Updating the front-end application code</span></span>
> * <span data-ttu-id="dd151-111">Vytvoření aktualizované image kontejneru</span><span class="sxs-lookup"><span data-stu-id="dd151-111">Creating an updated container image</span></span>
> * <span data-ttu-id="dd151-112">Nahrání image kontejneru do služby Azure Container Registry</span><span class="sxs-lookup"><span data-stu-id="dd151-112">Pushing the container image to Azure Container Registry</span></span>
> * <span data-ttu-id="dd151-113">Nasazení aktualizované image kontejneru</span><span class="sxs-lookup"><span data-stu-id="dd151-113">Deploying the updated container image</span></span>

<span data-ttu-id="dd151-114">V dalších kurzech se Operations Management Suite konfiguruje pro monitorování clusteru Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="dd151-114">In subsequent tutorials, Operations Management Suite is configured to monitor the Kubernetes cluster.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="dd151-115">Než začnete</span><span class="sxs-lookup"><span data-stu-id="dd151-115">Before you begin</span></span>

<span data-ttu-id="dd151-116">V předchozích kurzech se aplikace zabalila do image kontejneru, tato image se odeslala do Azure Container Registry a vytvořil se cluster Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="dd151-116">In previous tutorials, an application was packaged into a container image, the image uploaded to Azure Container Registry, and a Kubernetes cluster created.</span></span> <span data-ttu-id="dd151-117">Aplikace se potom spustila v tomto clusteru Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="dd151-117">The application was then run on the Kubernetes cluster.</span></span> 

<span data-ttu-id="dd151-118">Naklonovalo se také úložiště aplikace, které zahrnuje zdrojový kód aplikace, a předem se vytvořil soubor nástroje Docker Compose použitý v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="dd151-118">An application repository was also cloned which includes the application source code, and a pre-created Docker Compose file used in this tutorial.</span></span> <span data-ttu-id="dd151-119">Ověřte, že jste vytvořili klon úložiště a že jste změnili adresáře na klonovaný adresář.</span><span class="sxs-lookup"><span data-stu-id="dd151-119">Verify that you have created a clone of the repo, and that you have changed directories into the cloned directory.</span></span> <span data-ttu-id="dd151-120">Uvnitř je adresář nazvaný `azure-vote` a soubor s názvem `docker-compose.yml`.</span><span class="sxs-lookup"><span data-stu-id="dd151-120">Inside is a directory named `azure-vote` and a file named `docker-compose.yml`.</span></span>

<span data-ttu-id="dd151-121">Pokud jste tyto kroky nedokončili a chcete je sledovat, vraťte se ke [kurzu 1 – Vytváření imagí kontejneru](./container-service-tutorial-kubernetes-prepare-app.md).</span><span class="sxs-lookup"><span data-stu-id="dd151-121">If you haven't completed these steps, and want to follow along, return to [Tutorial 1 – Create container images](./container-service-tutorial-kubernetes-prepare-app.md).</span></span> 

## <a name="update-application"></a><span data-ttu-id="dd151-122">Aktualizace aplikace</span><span class="sxs-lookup"><span data-stu-id="dd151-122">Update application</span></span>

<span data-ttu-id="dd151-123">V tomto kurzu dojde ke změně aplikace a aktualizovaná aplikace se nasadí do clusteru Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="dd151-123">For this tutorial, a change is made to the application, and the updated application deployed to the Kubernetes cluster.</span></span> 

<span data-ttu-id="dd151-124">Zdrojový kód aplikace najdete v adresáři `azure-vote`.</span><span class="sxs-lookup"><span data-stu-id="dd151-124">The application source code can be found inside of the `azure-vote` directory.</span></span> <span data-ttu-id="dd151-125">Otevřete `config_file.cfg` soubor pomocí libovolného textového editoru nebo editoru kódu.</span><span class="sxs-lookup"><span data-stu-id="dd151-125">Open the `config_file.cfg` file with any code or text editor.</span></span> <span data-ttu-id="dd151-126">V tomto příkladu se používá `vi`.</span><span class="sxs-lookup"><span data-stu-id="dd151-126">In this example `vi` is used.</span></span>

```bash
vi azure-vote/azure-vote/config_file.cfg
```

<span data-ttu-id="dd151-127">Změňte hodnoty pro `VOTE1VALUE` a `VOTE2VALUE`, a potom soubor uložte.</span><span class="sxs-lookup"><span data-stu-id="dd151-127">Change the values for `VOTE1VALUE` and `VOTE2VALUE`, and then save the file.</span></span>

```bash
# UI Configurations
TITLE = 'Azure Voting App'
VOTE1VALUE = 'Blue'
VOTE2VALUE = 'Purple'
SHOWHOST = 'false'
```

<span data-ttu-id="dd151-128">Uložte soubor a zavřete ho.</span><span class="sxs-lookup"><span data-stu-id="dd151-128">Save and close the file.</span></span>

## <a name="update-container-image"></a><span data-ttu-id="dd151-129">Aktualizace image kontejneru</span><span class="sxs-lookup"><span data-stu-id="dd151-129">Update container image</span></span>

<span data-ttu-id="dd151-130">Pomocí příkazu [docker-compose](https://docs.docker.com/compose/) znovu vytvořte image front-endu a spusťte aktualizovanou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="dd151-130">Use [docker-compose](https://docs.docker.com/compose/) to re-create the front-end image and run the updated application.</span></span> <span data-ttu-id="dd151-131">Jako instrukce pro Docker Compose, že se má znovu vytvořit image aplikace, se použije argument `--build`.</span><span class="sxs-lookup"><span data-stu-id="dd151-131">The `--build` argument is used to instruct Docker Compose to re-create the application image.</span></span>

```bash
docker-compose up --build -d
```

## <a name="test-application-locally"></a><span data-ttu-id="dd151-132">Testování aplikace v místním prostředí</span><span class="sxs-lookup"><span data-stu-id="dd151-132">Test application locally</span></span>

<span data-ttu-id="dd151-133">Přejděte na http://localhost:8080 a prohlédněte si aktualizovanou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="dd151-133">Browse to http://localhost:8080 to see the updated application.</span></span>

![Obrázek clusteru Kubernetes v Azure](media/container-service-kubernetes-tutorials/vote-app-updated.png)

## <a name="tag-and-push-images"></a><span data-ttu-id="dd151-135">Označení a nahrání imagí</span><span class="sxs-lookup"><span data-stu-id="dd151-135">Tag and push images</span></span>

<span data-ttu-id="dd151-136">Označte image `azure-vote-front` pomocí názvu loginServer registru kontejneru.</span><span class="sxs-lookup"><span data-stu-id="dd151-136">Tag the `azure-vote-front` image with the loginServer of the container registry.</span></span> 

<span data-ttu-id="dd151-137">Název přihlašovacího serveru získáte pomocí příkazu [az acr list](/cli/azure/acr#list).</span><span class="sxs-lookup"><span data-stu-id="dd151-137">Get the login server name with the [az acr list](/cli/azure/acr#list) command.</span></span>

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

<span data-ttu-id="dd151-138">K označení image použijte [docker tag](https://docs.docker.com/engine/reference/commandline/tag/).</span><span class="sxs-lookup"><span data-stu-id="dd151-138">Use [docker tag](https://docs.docker.com/engine/reference/commandline/tag/) to tag the image.</span></span> <span data-ttu-id="dd151-139">Místo `<acrLoginServer>` použijte název přihlašovacího serveru služby Azure Container Registry nebo název hostitele veřejného registru.</span><span class="sxs-lookup"><span data-stu-id="dd151-139">Replace `<acrLoginServer>` with your Azure Container Registry login server name or public registry hostname.</span></span> <span data-ttu-id="dd151-140">Všimněte si také, že se verze image aktualizovala na `redis-v2`.</span><span class="sxs-lookup"><span data-stu-id="dd151-140">Also notice that the image version is updated to `redis-v2`.</span></span>

```bash
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:redis-v2
```

<span data-ttu-id="dd151-141">Pomocí příkazu [docker push](https://docs.docker.com/engine/reference/commandline/push/) odešlete image do registru.</span><span class="sxs-lookup"><span data-stu-id="dd151-141">Use [docker push](https://docs.docker.com/engine/reference/commandline/push/) to upload the image to your registry.</span></span> <span data-ttu-id="dd151-142">Místo `<acrLoginServer>` použijte název přihlašovacího serveru služby Azure Container Registry.</span><span class="sxs-lookup"><span data-stu-id="dd151-142">Replace `<acrLoginServer>` with your Azure Container Registry login server name.</span></span>

```bash
docker push <acrLoginServer>/azure-vote-front:redis-v2
```

## <a name="deploy-update-application"></a><span data-ttu-id="dd151-143">Nasazení aktualizované aplikace</span><span class="sxs-lookup"><span data-stu-id="dd151-143">Deploy update application</span></span>

<span data-ttu-id="dd151-144">K zajištění maximální doby provozu musí být spuštěno několik instancí podu aplikace.</span><span class="sxs-lookup"><span data-stu-id="dd151-144">To ensure maximum uptime, multiple instances of the application pod must be running.</span></span> <span data-ttu-id="dd151-145">Ověřte tuto konfiguraci pomocí příkazu [kubectl get pod](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get).</span><span class="sxs-lookup"><span data-stu-id="dd151-145">Verify this configuration with the [kubectl get pod](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) command.</span></span>

```bash
kubectl get pod
```

<span data-ttu-id="dd151-146">Výstup:</span><span class="sxs-lookup"><span data-stu-id="dd151-146">Output:</span></span>

```bash
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-217588096-5w632    1/1       Running   0          10m
azure-vote-front-233282510-b5pkz   1/1       Running   0          10m
azure-vote-front-233282510-dhrtr   1/1       Running   0          10m
azure-vote-front-233282510-pqbfk   1/1       Running   0          10m
```

<span data-ttu-id="dd151-147">Pokud nemáte několik podů se spuštěnou imagí azure-vote-front, škálujte nasazení `azure-vote-front`.</span><span class="sxs-lookup"><span data-stu-id="dd151-147">If you do not have multiple pods running the azure-vote-front image, scale the `azure-vote-front` deployment.</span></span>


```azurecli-interactive
kubectl scale --replicas=3 deployment/azure-vote-front
```

<span data-ttu-id="dd151-148">K aktualizaci aplikace použijte příkaz [kubectl set](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#set).</span><span class="sxs-lookup"><span data-stu-id="dd151-148">To update the application, use the [kubectl set](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#set) command.</span></span> <span data-ttu-id="dd151-149">Jako `<acrLoginServer>` použijte přihlašovací název serveru nebo hostitele vašeho registru kontejneru.</span><span class="sxs-lookup"><span data-stu-id="dd151-149">Update `<acrLoginServer>` with the login server or host name of your container registry.</span></span>

```azurecli-interactive
kubectl set image deployment azure-vote-front azure-vote-front=<acrLoginServer>/azure-vote-front:redis-v2
```

<span data-ttu-id="dd151-150">K monitorování nasazení použijte příkaz [kubectl get pod](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get).</span><span class="sxs-lookup"><span data-stu-id="dd151-150">To monitor the deployment, use the [kubectl get pod](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) command.</span></span> <span data-ttu-id="dd151-151">Při nasazení aktualizované aplikace se vaše pody ukončí a vytvoří se znovu s novou imagí kontejneru.</span><span class="sxs-lookup"><span data-stu-id="dd151-151">As the updated application is deployed, your pods are terminated and re-created with the new container image.</span></span>

```azurecli-interactive
kubectl get pod
```

<span data-ttu-id="dd151-152">Výstup:</span><span class="sxs-lookup"><span data-stu-id="dd151-152">Output:</span></span>

```bash
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-2978095810-gq9g0   1/1       Running   0          5m
azure-vote-front-1297194256-tpjlg   1/1       Running   0         1m
azure-vote-front-1297194256-tptnx   1/1       Running   0         5m
azure-vote-front-1297194256-zktw9   1/1       Terminating   0         1m
```

## <a name="test-updated-application"></a><span data-ttu-id="dd151-153">Otestování aktualizované aplikace</span><span class="sxs-lookup"><span data-stu-id="dd151-153">Test updated application</span></span>

<span data-ttu-id="dd151-154">Získejte externí IP adresu služby `azure-vote-front`.</span><span class="sxs-lookup"><span data-stu-id="dd151-154">Get the external IP address of the `azure-vote-front` service.</span></span>

```azurecli-interactive
kubectl get service azure-vote-front
```

<span data-ttu-id="dd151-155">Přejděte na tuto IP adresu a prohlédněte si aktualizovanou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="dd151-155">Browse to the IP address to see the updated application.</span></span>

![Obrázek clusteru Kubernetes v Azure](media/container-service-kubernetes-tutorials/vote-app-updated-external.png)

## <a name="next-steps"></a><span data-ttu-id="dd151-157">Další postup</span><span class="sxs-lookup"><span data-stu-id="dd151-157">Next steps</span></span>

<span data-ttu-id="dd151-158">V tomto kurzu jste aktualizovali aplikaci a zavedli tuto aktualizaci do clusteru Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="dd151-158">In this tutorial, you updated an application and rolled out this update to a Kubernetes cluster.</span></span> <span data-ttu-id="dd151-159">Dokončili jste následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="dd151-159">The following tasks were completed:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="dd151-160">Aktualizace aplikačního kódu front-endu</span><span class="sxs-lookup"><span data-stu-id="dd151-160">Updated the front-end application code</span></span>
> * <span data-ttu-id="dd151-161">Vytvoření aktualizované image kontejneru</span><span class="sxs-lookup"><span data-stu-id="dd151-161">Created an updated container image</span></span>
> * <span data-ttu-id="dd151-162">Nahrání image kontejneru do služby Azure Container Registry</span><span class="sxs-lookup"><span data-stu-id="dd151-162">Pushed the container image to Azure Container Registry</span></span>
> * <span data-ttu-id="dd151-163">Nasazení aktualizované aplikace</span><span class="sxs-lookup"><span data-stu-id="dd151-163">Deployed the updated application</span></span>

<span data-ttu-id="dd151-164">V dalším kurzu se dozvíte, jak monitorovat Kubernetes s využitím sady Operations Management Suite.</span><span class="sxs-lookup"><span data-stu-id="dd151-164">Advance to the next tutorial to learn about how to monitor Kubernetes with Operations Management Suite.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="dd151-165">Monitorování Kubernetes pomocí OMS</span><span class="sxs-lookup"><span data-stu-id="dd151-165">Monitor Kubernetes with OMS</span></span>](./container-service-tutorial-kubernetes-monitor.md)