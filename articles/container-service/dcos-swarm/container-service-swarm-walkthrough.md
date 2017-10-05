---
title: "Rychlý start – Cluster Azure Docker Swarm pro Linux | Dokumentace Microsoftu"
description: "Rychle se naučíte, jak pomocí Azure CLI vytvořit cluster Docker Swarm pro kontejnery Linuxu ve službě Azure Container Service."
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service, Docker, Swarm
keywords: 
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/14/2017
ms.author: nepeters
ms.custom: 
ms.openlocfilehash: 1d10c347795227ed056a95d1bcd4aff82af7b876
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="deploy-docker-swarm-cluster"></a><span data-ttu-id="1d90d-103">Nasazení clusteru Docker Swarm</span><span class="sxs-lookup"><span data-stu-id="1d90d-103">Deploy Docker Swarm cluster</span></span>

<span data-ttu-id="1d90d-104">V tomto rychlém startu se nasadí cluster Docker Swarm pomocí Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="1d90d-104">In this quick start, a Docker Swarm cluster is deployed using the Azure CLI.</span></span> <span data-ttu-id="1d90d-105">Následně se na tomto clusteru nasadí a spustí vícekontejnerová aplikace skládající se z webu front-end a instance Redis.</span><span class="sxs-lookup"><span data-stu-id="1d90d-105">A multi-container application consisting of web front end and a Redis instance is then deployed and run on the cluster.</span></span> <span data-ttu-id="1d90d-106">Po dokončení bude aplikace přístupná přes internet.</span><span class="sxs-lookup"><span data-stu-id="1d90d-106">Once completed, the application is accessible over the internet.</span></span>

<span data-ttu-id="1d90d-107">Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.</span><span class="sxs-lookup"><span data-stu-id="1d90d-107">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

<span data-ttu-id="1d90d-108">Tento rychlý start vyžaduje použití Azure CLI verze 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="1d90d-108">This quickstart requires that you are running the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="1d90d-109">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="1d90d-109">Run `az --version` to find the version.</span></span> <span data-ttu-id="1d90d-110">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="1d90d-110">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span>

## <a name="create-a-resource-group"></a><span data-ttu-id="1d90d-111">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="1d90d-111">Create a resource group</span></span>

<span data-ttu-id="1d90d-112">Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="1d90d-112">Create a resource group with the [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="1d90d-113">Skupina prostředků Azure je logická skupina, ve které se nasazují a spravují prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="1d90d-113">An Azure resource group is a logical group in which Azure resources are deployed and managed.</span></span>

<span data-ttu-id="1d90d-114">Následující příklad vytvoří skupinu prostředků *myResourceGroup* v umístění *westus*.</span><span class="sxs-lookup"><span data-stu-id="1d90d-114">The following example creates a resource group named *myResourceGroup* in the *westus* location.</span></span>

```azurecli-interactive
az group create --name myResourceGroup --location westus
```

<span data-ttu-id="1d90d-115">Výstup:</span><span class="sxs-lookup"><span data-stu-id="1d90d-115">Output:</span></span>

```json
{
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup",
  "location": "westcentralus",
  "managedBy": null,
  "name": "myResourceGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

## <a name="create-docker-swarm-cluster"></a><span data-ttu-id="1d90d-116">Vytvoření clusteru Docker Swarm</span><span class="sxs-lookup"><span data-stu-id="1d90d-116">Create Docker Swarm cluster</span></span>

<span data-ttu-id="1d90d-117">Vytvořte cluster Docker Swarm ve službě Azure Container Service pomocí příkazu [az acs create](/cli/azure/acs#create).</span><span class="sxs-lookup"><span data-stu-id="1d90d-117">Create a Docker Swarm cluster in Azure Container Service with the [az acs create](/cli/azure/acs#create) command.</span></span> 

<span data-ttu-id="1d90d-118">Následující příklad vytvoří cluster *mySwarmCluster* s jedním hlavním linuxovým uzlem a třemi agentskými linuxovými uzly.</span><span class="sxs-lookup"><span data-stu-id="1d90d-118">The following example creates a cluster named *mySwarmCluster* with one Linux master node and three Linux agent nodes.</span></span>

```azurecli-interactive
az acs create --name mySwarmCluster --orchestrator-type Swarm --resource-group myResourceGroup --generate-ssh-keys
```

<span data-ttu-id="1d90d-119">Po několika minutách se příkaz dokončí a vrátí informace o clusteru ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="1d90d-119">After several minutes, the command completes and returns json formatted information about the cluster.</span></span>

## <a name="connect-to-the-cluster"></a><span data-ttu-id="1d90d-120">Připojení ke clusteru</span><span class="sxs-lookup"><span data-stu-id="1d90d-120">Connect to the cluster</span></span>

<span data-ttu-id="1d90d-121">V průběhu tohoto rychlého startu budete potřebovat IP adresu hlavního uzlu Dockeru Swarm i fondu agentských uzlů Dockeru.</span><span class="sxs-lookup"><span data-stu-id="1d90d-121">Throughout this quick start, you need the IP address of both the Docker Swarm master and the Docker agent pool.</span></span> <span data-ttu-id="1d90d-122">Spusťte následující příkaz, který vrátí obě IP adresy.</span><span class="sxs-lookup"><span data-stu-id="1d90d-122">Run the following command to return both IP addresses.</span></span>


```bash
az network public-ip list --resource-group myResourceGroup --query '[*].{Name:name,IPAddress:ipAddress}' -o table
```

<span data-ttu-id="1d90d-123">Výstup:</span><span class="sxs-lookup"><span data-stu-id="1d90d-123">Output:</span></span>

```bash
Name                                                                 IPAddress
-------------------------------------------------------------------  -------------
swarmm-agent-ip-myswarmcluster-myresourcegroup-d5b9d4agent-66066781  52.179.23.131
swarmm-master-ip-myswarmcluster-myresourcegroup-d5b9d4mgmt-66066781  52.141.37.199
```

<span data-ttu-id="1d90d-124">Vytvořte tunel SSH k hlavnímu uzlu Swarm.</span><span class="sxs-lookup"><span data-stu-id="1d90d-124">Create an SSH tunnel to the Swarm master.</span></span> <span data-ttu-id="1d90d-125">Nahraďte `IPAddress` IP adresou hlavního uzlu Swarm.</span><span class="sxs-lookup"><span data-stu-id="1d90d-125">Replace `IPAddress` with the IP address of the Swarm master.</span></span>

```bash
ssh -p 2200 -fNL 2375:localhost:2375 azureuser@IPAddress
```

<span data-ttu-id="1d90d-126">Nastavte proměnnou prostředí `DOCKER_HOST`.</span><span class="sxs-lookup"><span data-stu-id="1d90d-126">Set the `DOCKER_HOST` environment variable.</span></span> <span data-ttu-id="1d90d-127">To vám umožní spouštět příkazy Dockeru pro Docker Swarm, aniž byste museli zadávat název hostitele.</span><span class="sxs-lookup"><span data-stu-id="1d90d-127">This allows you to run docker commands against the Docker Swarm without having to specify the name of the host.</span></span>

```bash
export DOCKER_HOST=:2375
```

<span data-ttu-id="1d90d-128">Nyní jste připraveni spustit služby Dockeru v Dockeru Swarm.</span><span class="sxs-lookup"><span data-stu-id="1d90d-128">You are now ready to run Docker services on the Docker Swarm.</span></span>


## <a name="run-the-application"></a><span data-ttu-id="1d90d-129">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="1d90d-129">Run the application</span></span>

<span data-ttu-id="1d90d-130">Vytvořte soubor `docker-compose.yaml` a zkopírujte do něj následující obsah.</span><span class="sxs-lookup"><span data-stu-id="1d90d-130">Create a file named `docker-compose.yaml` and copy the following content into it.</span></span>

```yaml
version: '3'
services:
  azure-vote-back:
    image: redis
    container_name: azure-vote-back
    ports:
        - "6379:6379"

  azure-vote-front:
    image: microsoft/azure-vote-front:redis-v1
    container_name: azure-vote-front
    environment:
      REDIS: azure-vote-back
    ports:
        - "80:80"
```

<span data-ttu-id="1d90d-131">Spuštěním následujícího příkazu vytvořte službu Azure Vote.</span><span class="sxs-lookup"><span data-stu-id="1d90d-131">Run the following command to create the Azure Vote service.</span></span>

```bash
docker-compose up -d
```

<span data-ttu-id="1d90d-132">Výstup:</span><span class="sxs-lookup"><span data-stu-id="1d90d-132">Output:</span></span>

```bash
Creating network "user_default" with the default driver
Pulling azure-vote-front (microsoft/azure-vote-front:redis-v1)...
swarm-agent-EE873B23000005: Pulling microsoft/azure-vote-front:redis-v1...
swarm-agent-EE873B23000004: Pulling microsoft/azure-vote-front:redis-v1... : downloaded
Pulling azure-vote-back (redis:latest)...
swarm-agent-EE873B23000004: Pulling redis:latest... : downloaded
Creating azure-vote-front ... 
Creating azure-vote-back ... 
Creating azure-vote-front
Creating azure-vote-back ...
```

## <a name="test-the-application"></a><span data-ttu-id="1d90d-133">Testování aplikace</span><span class="sxs-lookup"><span data-stu-id="1d90d-133">Test the application</span></span>

<span data-ttu-id="1d90d-134">Přejděte na IP adresu fondu agentských uzlů Swarm a otestujte aplikaci Azure Vote.</span><span class="sxs-lookup"><span data-stu-id="1d90d-134">Browse to the IP address of the Swarm agent pool to test out the Azure Vote application.</span></span>

![Obrázek přechodu na aplikaci Azure Vote](media/container-service-docker-swarm-mode-walkthrough/azure-vote.png)

## <a name="delete-cluster"></a><span data-ttu-id="1d90d-136">Odstranění clusteru</span><span class="sxs-lookup"><span data-stu-id="1d90d-136">Delete cluster</span></span>
<span data-ttu-id="1d90d-137">Pokud už cluster nepotřebujete, můžete k odebrání skupiny prostředků, služby kontejneru a všech souvisejících prostředků použít příkaz [az group delete](/cli/azure/group#delete).</span><span class="sxs-lookup"><span data-stu-id="1d90d-137">When the cluster is no longer needed, you can use the [az group delete](/cli/azure/group#delete) command to remove the resource group, container service, and all related resources.</span></span>

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```

## <a name="get-the-code"></a><span data-ttu-id="1d90d-138">Získání kódu</span><span class="sxs-lookup"><span data-stu-id="1d90d-138">Get the code</span></span>

<span data-ttu-id="1d90d-139">V tomto rychlém startu se k vytvoření služby Docker použily předem vytvořené image kontejneru.</span><span class="sxs-lookup"><span data-stu-id="1d90d-139">In this quick start, pre-created container images have been used to create a Docker service.</span></span> <span data-ttu-id="1d90d-140">Související kód aplikace, soubor Dockerfile a soubor Compose jsou k dispozici na GitHubu.</span><span class="sxs-lookup"><span data-stu-id="1d90d-140">The related application code, Dockerfile, and Compose file are available on GitHub.</span></span>

[<span data-ttu-id="1d90d-141">https://github.com/Azure-Samples/azure-voting-app-redis</span><span class="sxs-lookup"><span data-stu-id="1d90d-141">https://github.com/Azure-Samples/azure-voting-app-redis</span></span>](https://github.com/Azure-Samples/azure-voting-app-redis.git)

## <a name="next-steps"></a><span data-ttu-id="1d90d-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1d90d-142">Next steps</span></span>

<span data-ttu-id="1d90d-143">V tomto rychlém startu jste nasadili cluster Docker Swarm a do něj jste nasadili vícekontejnerovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1d90d-143">In this quick start, you deployed a Docker Swarm cluster and deployed a multi-container application to it.</span></span>

<span data-ttu-id="1d90d-144">Informace o integraci Dockeru Swarm s Visual Studio Team Services najdete v tématu věnovaném průběžné integraci a doručování s využitím Dockeru Swarm a VSTS.</span><span class="sxs-lookup"><span data-stu-id="1d90d-144">To learn about integrating Docker warm with Visual Studio Team Services, continue to the CI/CD with Docker Swarm and VSTS.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="1d90d-145">CI/CD s Docker Swarm a VSTS</span><span class="sxs-lookup"><span data-stu-id="1d90d-145">CI/CD with Docker Swarm and VSTS</span></span>](./container-service-docker-swarm-setup-ci-cd.md)