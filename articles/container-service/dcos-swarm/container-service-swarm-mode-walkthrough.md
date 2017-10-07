---
title: aaaQuickstart - clusteru Azure Docker CE pro Linux | Microsoft Docs
description: "Naučte se rychle toocreate clusteru Docker CE Linux kontejnerů v Azure Container Service s hello rozhraní příkazového řádku Azure."
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
ms.date: 08/25/2017
ms.author: nepeters
ms.custom: 
ms.openlocfilehash: 6c26c12ed085ec379c3486095a5fa51379afc5a2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-docker-ce-cluster"></a><span data-ttu-id="9bbad-103">Nasazení clusteru Docker CE</span><span class="sxs-lookup"><span data-stu-id="9bbad-103">Deploy Docker CE cluster</span></span>

<span data-ttu-id="9bbad-104">V této úvodní clusteru Docker CE je nasazená pomocí hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="9bbad-104">In this quick start, a Docker CE cluster is deployed using hello Azure CLI.</span></span> <span data-ttu-id="9bbad-105">Aplikace více kontejneru, který se skládá z webového front-endu a instanci Redis nasazení a poté běží na clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="9bbad-105">A multi-container application consisting of web front end and a Redis instance is then deployed and run on hello cluster.</span></span> <span data-ttu-id="9bbad-106">Po dokončení aplikace hello je přístupné prostřednictvím Internetu hello.</span><span class="sxs-lookup"><span data-stu-id="9bbad-106">Once completed, hello application is accessible over hello internet.</span></span>

<span data-ttu-id="9bbad-107">Docker CE ve službě Azure Container Service je ve verzi Preview a **neměl by se používat pro produkční úlohy**.</span><span class="sxs-lookup"><span data-stu-id="9bbad-107">Docker CE on Azure Container Service is in preview and **should not be used for production workloads**.</span></span>

<span data-ttu-id="9bbad-108">Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.</span><span class="sxs-lookup"><span data-stu-id="9bbad-108">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

<span data-ttu-id="9bbad-109">Pokud zvolíte tooinstall a místně pomocí hello rozhraní příkazového řádku, tento rychlý start vyžaduje, že používáte verzi rozhraní příkazového řádku Azure hello verze 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="9bbad-109">If you choose tooinstall and use hello CLI locally, this quickstart requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="9bbad-110">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="9bbad-110">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="9bbad-111">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="9bbad-111">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span>

## <a name="create-a-resource-group"></a><span data-ttu-id="9bbad-112">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="9bbad-112">Create a resource group</span></span>

<span data-ttu-id="9bbad-113">Vytvořte skupinu prostředků s hello [vytvořit skupinu az](/cli/azure/group#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="9bbad-113">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="9bbad-114">Skupina prostředků Azure je logická skupina, ve které se nasazují a spravují prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="9bbad-114">An Azure resource group is a logical group in which Azure resources are deployed and managed.</span></span>

<span data-ttu-id="9bbad-115">Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *ukwest* umístění.</span><span class="sxs-lookup"><span data-stu-id="9bbad-115">hello following example creates a resource group named *myResourceGroup* in hello *ukwest* location.</span></span>

```azurecli-interactive
az group create --name myResourceGroup --location ukwest
```

<span data-ttu-id="9bbad-116">Výstup:</span><span class="sxs-lookup"><span data-stu-id="9bbad-116">Output:</span></span>

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

## <a name="create-docker-swarm-cluster"></a><span data-ttu-id="9bbad-117">Vytvoření clusteru Docker Swarm</span><span class="sxs-lookup"><span data-stu-id="9bbad-117">Create Docker Swarm cluster</span></span>

<span data-ttu-id="9bbad-118">Vytvoření clusteru Docker CE v Azure Container Service s hello [vytvořit acs az](/cli/azure/acs#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="9bbad-118">Create a Docker CE cluster in Azure Container Service with hello [az acs create](/cli/azure/acs#create) command.</span></span> 

<span data-ttu-id="9bbad-119">Hello následující příklad vytvoří cluster s názvem *mySwarmCluster* s Linuxem jeden hlavní uzel a tři uzly Linux agent.</span><span class="sxs-lookup"><span data-stu-id="9bbad-119">hello following example creates a cluster named *mySwarmCluster* with one Linux master node and three Linux agent nodes.</span></span>

```azurecli-interactive
az acs create --name mySwarmCluster --orchestrator-type dockerce --resource-group myResourceGroup --generate-ssh-keys
```

<span data-ttu-id="9bbad-120">Po několika minutách hello příkaz dokončí a vrátí formátu json informace o clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="9bbad-120">After several minutes, hello command completes and returns json formatted information about hello cluster.</span></span>

## <a name="connect-toohello-cluster"></a><span data-ttu-id="9bbad-121">Připojte toohello cluster</span><span class="sxs-lookup"><span data-stu-id="9bbad-121">Connect toohello cluster</span></span>

<span data-ttu-id="9bbad-122">V rámci této úvodní musíte hello plně kvalifikovaný název domény hello Docker Swarm hlavní a hello Docker agenta fondu.</span><span class="sxs-lookup"><span data-stu-id="9bbad-122">Throughout this quick start, you need hello FQDN of both hello Docker Swarm master and hello Docker agent pool.</span></span> <span data-ttu-id="9bbad-123">Spusťte následující příkaz tooreturn hello obě hello hlavní a agent plně kvalifikované názvy domén.</span><span class="sxs-lookup"><span data-stu-id="9bbad-123">Run hello following command tooreturn both hello master and agent FQDNs.</span></span>


```bash
az acs list --resource-group myResourceGroup --query '[*].{Master:masterProfile.fqdn,Agent:agentPoolProfiles[0].fqdn}' -o table
```

<span data-ttu-id="9bbad-124">Výstup:</span><span class="sxs-lookup"><span data-stu-id="9bbad-124">Output:</span></span>

```bash
Master                                                               Agent
-------------------------------------------------------------------  --------------------------------------------------------------------
myswarmcluster-myresourcegroup-d5b9d4mgmt.ukwest.cloudapp.azure.com  myswarmcluster-myresourcegroup-d5b9d4agent.ukwest.cloudapp.azure.com
```

<span data-ttu-id="9bbad-125">Vytvořte hlavní Swarm toohello tunelového propojení SSH.</span><span class="sxs-lookup"><span data-stu-id="9bbad-125">Create an SSH tunnel toohello Swarm master.</span></span> <span data-ttu-id="9bbad-126">Nahraďte `MasterFQDN` s adresou hello plně kvalifikovaný název domény hlavního serveru Swarm hello.</span><span class="sxs-lookup"><span data-stu-id="9bbad-126">Replace `MasterFQDN` with hello FQDN address of hello Swarm master.</span></span>

```bash
ssh -p 2200 -fNL localhost:2374:/var/run/docker.sock azureuser@MasterFQDN
```

<span data-ttu-id="9bbad-127">Sada hello `DOCKER_HOST` proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="9bbad-127">Set hello `DOCKER_HOST` environment variable.</span></span> <span data-ttu-id="9bbad-128">To vám umožní toorun docker příkazy proti hello Docker Swarm bez nutnosti toospecify hello název hostitele hello.</span><span class="sxs-lookup"><span data-stu-id="9bbad-128">This allows you toorun docker commands against hello Docker Swarm without having toospecify hello name of hello host.</span></span>

```bash
export DOCKER_HOST=localhost:2374
```

<span data-ttu-id="9bbad-129">Nyní je připraven toorun Docker služeb v hello Docker Swarm.</span><span class="sxs-lookup"><span data-stu-id="9bbad-129">You are now ready toorun Docker services on hello Docker Swarm.</span></span>


## <a name="run-hello-application"></a><span data-ttu-id="9bbad-130">Spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="9bbad-130">Run hello application</span></span>

<span data-ttu-id="9bbad-131">Vytvořte soubor s názvem `azure-vote.yaml` a kopírování hello do něj následující obsah.</span><span class="sxs-lookup"><span data-stu-id="9bbad-131">Create a file named `azure-vote.yaml` and copy hello following content into it.</span></span>


```yaml
version: '3'
services:
  azure-vote-back:
    image: redis
    ports:
        - "6379:6379"

  azure-vote-front:
    image: microsoft/azure-vote-front:redis-v1
    environment:
      REDIS: azure-vote-back
    ports:
        - "80:80"
```

<span data-ttu-id="9bbad-132">Spustit hello [docker zásobníku nasazení](https://docs.docker.com/engine/reference/commandline/stack_deploy/) příkaz toocreate hello Azure hlas služby.</span><span class="sxs-lookup"><span data-stu-id="9bbad-132">Run hello [docker stack deploy](https://docs.docker.com/engine/reference/commandline/stack_deploy/) command toocreate hello Azure Vote service.</span></span>

```bash
docker stack deploy azure-vote --compose-file azure-vote.yaml
```

<span data-ttu-id="9bbad-133">Výstup:</span><span class="sxs-lookup"><span data-stu-id="9bbad-133">Output:</span></span>

```bash
Creating network azure-vote_default
Creating service azure-vote_azure-vote-back
Creating service azure-vote_azure-vote-front
```

<span data-ttu-id="9bbad-134">Použití hello [docker zásobníku ps](https://docs.docker.com/engine/reference/commandline/stack_ps/) příkaz tooreturn hello stav nasazení aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="9bbad-134">Use hello [docker stack ps](https://docs.docker.com/engine/reference/commandline/stack_ps/) command tooreturn hello deployment status of hello application.</span></span>

```bash
docker stack ps azure-vote
```

<span data-ttu-id="9bbad-135">Jednou hello `CURRENT STATE` každé služby je `Running`, hello aplikace je připravena.</span><span class="sxs-lookup"><span data-stu-id="9bbad-135">Once hello `CURRENT STATE` of each service is `Running`, hello application is ready.</span></span>

```bash
ID                  NAME                            IMAGE                                 NODE                               DESIRED STATE       CURRENT STATE                ERROR               PORTS
tnklkv3ogu3i        azure-vote_azure-vote-front.1   microsoft/azure-vote-front:redis-v1   swarmm-agentpool0-66066781000004   Running             Running 5 seconds ago                            
lg99i4hy68r9        azure-vote_azure-vote-back.1    redis:latest                          swarmm-agentpool0-66066781000002   Running             Running about a minute ago
```

## <a name="test-hello-application"></a><span data-ttu-id="9bbad-136">Testování aplikace hello</span><span class="sxs-lookup"><span data-stu-id="9bbad-136">Test hello application</span></span>

<span data-ttu-id="9bbad-137">Procházejte toohello plně kvalifikovaný název domény hello Swarm agenta fondu tootest out hello hlas Azure aplikace.</span><span class="sxs-lookup"><span data-stu-id="9bbad-137">Browse toohello FQDN of hello Swarm agent pool tootest out hello Azure Vote application.</span></span>

![Obrázek procházení tooAzure hlas](media/container-service-docker-swarm-mode-walkthrough/azure-vote.png)

## <a name="delete-cluster"></a><span data-ttu-id="9bbad-139">Odstranění clusteru</span><span class="sxs-lookup"><span data-stu-id="9bbad-139">Delete cluster</span></span>
<span data-ttu-id="9bbad-140">Pokud hello cluster je již nepotřebujete, můžete použít hello [odstranění skupiny az](/cli/azure/group#delete) příkaz skupiny prostředků hello tooremove, container service a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="9bbad-140">When hello cluster is no longer needed, you can use hello [az group delete](/cli/azure/group#delete) command tooremove hello resource group, container service, and all related resources.</span></span>

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```

## <a name="get-hello-code"></a><span data-ttu-id="9bbad-141">Získat kód hello</span><span class="sxs-lookup"><span data-stu-id="9bbad-141">Get hello code</span></span>

<span data-ttu-id="9bbad-142">V této úvodní předem vytvořené kontejneru bitové kopie byly použité toocreate Docker služby.</span><span class="sxs-lookup"><span data-stu-id="9bbad-142">In this quick start, pre-created container images have been used toocreate a Docker service.</span></span> <span data-ttu-id="9bbad-143">Hello související s kódu aplikace, soubor Docker, a soubor vytvářené jsou dostupné na Githubu.</span><span class="sxs-lookup"><span data-stu-id="9bbad-143">hello related application code, Dockerfile, and Compose file are available on GitHub.</span></span>

[<span data-ttu-id="9bbad-144">https://github.com/Azure-Samples/azure-voting-app-redis</span><span class="sxs-lookup"><span data-stu-id="9bbad-144">https://github.com/Azure-Samples/azure-voting-app-redis</span></span>](https://github.com/Azure-Samples/azure-voting-app-redis.git)

## <a name="next-steps"></a><span data-ttu-id="9bbad-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9bbad-145">Next steps</span></span>

<span data-ttu-id="9bbad-146">V této úvodní nasazení clusteru Docker Swarm a nasadit aplikace s více kontejnerů tooit.</span><span class="sxs-lookup"><span data-stu-id="9bbad-146">In this quick start, you deployed a Docker Swarm cluster and deployed a multi-container application tooit.</span></span>

<span data-ttu-id="9bbad-147">toolearn o Docker záložním integraci s Visual Studio Team Services, pokračovat toohello CI/CD s Docker Swarm a služby VSTS.</span><span class="sxs-lookup"><span data-stu-id="9bbad-147">toolearn about integrating Docker warm with Visual Studio Team Services, continue toohello CI/CD with Docker Swarm and VSTS.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="9bbad-148">CI/CD s Docker Swarm a VSTS</span><span class="sxs-lookup"><span data-stu-id="9bbad-148">CI/CD with Docker Swarm and VSTS</span></span>](./container-service-docker-swarm-setup-ci-cd.md)