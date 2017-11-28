---
title: "aaaDeploy cluster kontejner Docker - rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Nasazení řešení Kubernetes, DC/OS nebo Docker Swarm ve službě Azure Container Service pomocí Azure CLI 2.0"
services: container-service
documentationcenter: 
author: sauryadas
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: 
ms.assetid: 8da267e8-2aeb-4c24-9a7a-65bdca3a82d6
ms.service: container-service
ms.devlang: azurecli
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/01/2017
ms.author: saudas
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: cdfa4ce69de343dcc7070bc2c58b5132c4062084
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-docker-container-hosting-solution-using-hello-azure-cli-20"></a><span data-ttu-id="2d632-103">Nasazení řešení pomocí Azure CLI 2.0 hello hostování kontejner Docker</span><span class="sxs-lookup"><span data-stu-id="2d632-103">Deploy a Docker container hosting solution using hello Azure CLI 2.0</span></span>

<span data-ttu-id="2d632-104">Použití hello `az acs` příkazů v toocreate hello Azure CLI 2.0 a spravovat clustery v Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="2d632-104">Use hello `az acs` commands in hello Azure CLI 2.0 toocreate and manage clusters in Azure Container Service.</span></span> <span data-ttu-id="2d632-105">Můžete také nasazení clusteru Azure Container Service pomocí hello [portál Azure](container-service-deployment.md) nebo hello rozhraní API Správce Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="2d632-105">You can also deploy an Azure Container Service cluster by using hello [Azure portal](container-service-deployment.md) or hello Azure Container Service APIs.</span></span>

<span data-ttu-id="2d632-106">Nápovědu k `az acs` příkazy, předat hello `-h` parametr tooany příkaz.</span><span class="sxs-lookup"><span data-stu-id="2d632-106">For help on `az acs` commands, pass hello `-h` parameter tooany command.</span></span> <span data-ttu-id="2d632-107">Například: `az acs create -h`.</span><span class="sxs-lookup"><span data-stu-id="2d632-107">For example: `az acs create -h`.</span></span>



## <a name="prerequisites"></a><span data-ttu-id="2d632-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2d632-108">Prerequisites</span></span>
<span data-ttu-id="2d632-109">toocreate clusteru Azure Container Service pomocí hello 2.0 rozhraní příkazového řádku Azure, musíte:</span><span class="sxs-lookup"><span data-stu-id="2d632-109">toocreate an Azure Container Service cluster using hello Azure CLI 2.0, you must:</span></span>
* <span data-ttu-id="2d632-110">účet Azure ([získejte bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/)),</span><span class="sxs-lookup"><span data-stu-id="2d632-110">have an Azure account ([get a free trial](https://azure.microsoft.com/pricing/free-trial/))</span></span>
* <span data-ttu-id="2d632-111">instalaci a nastavení hello [2.0 rozhraní příkazového řádku Azure](/cli/azure/install-az-cli2)</span><span class="sxs-lookup"><span data-stu-id="2d632-111">have installed and set up hello [Azure CLI 2.0](/cli/azure/install-az-cli2)</span></span>

## <a name="get-started"></a><span data-ttu-id="2d632-112">Začínáme</span><span class="sxs-lookup"><span data-stu-id="2d632-112">Get started</span></span> 
### <a name="log-in-tooyour-account"></a><span data-ttu-id="2d632-113">Přihlaste se tooyour účtu</span><span class="sxs-lookup"><span data-stu-id="2d632-113">Log in tooyour account</span></span>
```azurecli
az login 
```

<span data-ttu-id="2d632-114">Postupujte podle pokynů toolog hello v interaktivně.</span><span class="sxs-lookup"><span data-stu-id="2d632-114">Follow hello prompts toolog in interactively.</span></span> <span data-ttu-id="2d632-115">Ostatní metody toolog v, najdete v části [Začínáme s Azure CLI 2.0](/cli/azure/get-started-with-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="2d632-115">For other methods toolog in, see [Get started with Azure CLI 2.0](/cli/azure/get-started-with-az-cli2).</span></span>

### <a name="set-your-azure-subscription"></a><span data-ttu-id="2d632-116">Nastavení předplatného Azure</span><span class="sxs-lookup"><span data-stu-id="2d632-116">Set your Azure subscription</span></span>

<span data-ttu-id="2d632-117">Pokud máte více než jedno předplatné, nastavte hello výchozí předplatné.</span><span class="sxs-lookup"><span data-stu-id="2d632-117">If you have more than one Azure subscription, set hello default subscription.</span></span> <span data-ttu-id="2d632-118">Například:</span><span class="sxs-lookup"><span data-stu-id="2d632-118">For example:</span></span>

```
az account set --subscription "f66xxxxx-xxxx-xxxx-xxx-zgxxxx33cha5"
```


### <a name="create-a-resource-group"></a><span data-ttu-id="2d632-119">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="2d632-119">Create a resource group</span></span>
<span data-ttu-id="2d632-120">Doporučujeme vytvořit skupinu prostředků pro každý cluster.</span><span class="sxs-lookup"><span data-stu-id="2d632-120">We recommend that you create a resource group for every cluster.</span></span> <span data-ttu-id="2d632-121">Zadejte oblast Azure, ve které je Azure Container Service [k dispozici](https://azure.microsoft.com/en-us/regions/services/).</span><span class="sxs-lookup"><span data-stu-id="2d632-121">Specify an Azure region in which Azure Container Service is [available](https://azure.microsoft.com/en-us/regions/services/).</span></span> <span data-ttu-id="2d632-122">Například:</span><span class="sxs-lookup"><span data-stu-id="2d632-122">For example:</span></span>

```azurecli
az group create -n acsrg1 -l "westus"
```
<span data-ttu-id="2d632-123">Výstup je podobné toohello následující:</span><span class="sxs-lookup"><span data-stu-id="2d632-123">Output is similar toohello following:</span></span>

![Vytvoření skupiny prostředků](./media/container-service-create-acs-cluster-cli/rg-create.png)


## <a name="create-an-azure-container-service-cluster"></a><span data-ttu-id="2d632-125">Vytvoření clusteru Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="2d632-125">Create an Azure Container Service cluster</span></span>

<span data-ttu-id="2d632-126">toocreate cluster, použijte `az acs create`.</span><span class="sxs-lookup"><span data-stu-id="2d632-126">toocreate a cluster, use `az acs create`.</span></span>
<span data-ttu-id="2d632-127">Název pro hello cluster a hello název skupiny prostředků hello vytvořili v předchozím kroku hello jsou povinné parametry.</span><span class="sxs-lookup"><span data-stu-id="2d632-127">A name for hello cluster and hello name of hello resource group created in hello previous step are mandatory parameters.</span></span> 

<span data-ttu-id="2d632-128">Další vstupní hodnoty jsou nastavené hodnoty toodefault (viz následující obrazovka hello) není-li přepsat pomocí jejich odpovídajících přepínačů.</span><span class="sxs-lookup"><span data-stu-id="2d632-128">Other inputs are set toodefault values (see hello following screen) unless overwritten using their respective switches.</span></span> <span data-ttu-id="2d632-129">Hello orchestrator je třeba nastavit ve výchozím nastavení tooDC/OS.</span><span class="sxs-lookup"><span data-stu-id="2d632-129">For example, hello orchestrator is set by default tooDC/OS.</span></span> <span data-ttu-id="2d632-130">A pokud nezadáte jeden, předpony názvu DNS je vytvořena na základě názvu clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="2d632-130">And if you don't specify one, a DNS name prefix is created based on hello cluster name.</span></span>

![použití příkazu az acs create](./media/container-service-create-acs-cluster-cli/create-help.png)


### <a name="quick-acs-create-using-defaults"></a><span data-ttu-id="2d632-132">Rychlý příkaz `acs create` s využitím výchozích hodnot</span><span class="sxs-lookup"><span data-stu-id="2d632-132">Quick `acs create` using defaults</span></span>
<span data-ttu-id="2d632-133">Pokud máte soubor veřejného klíče SSH RSA `id_rsa.pub` ve výchozím umístění hello (nebo jeden pro vytvoření [OS X a Linux](../../virtual-machines/linux/mac-create-ssh-keys.md) nebo [Windows](../../virtual-machines/linux/ssh-from-windows.md)), použijte příkaz jako hello následující:</span><span class="sxs-lookup"><span data-stu-id="2d632-133">If you have an SSH RSA public key file `id_rsa.pub` in hello default location (or created one for [OS X and Linux](../../virtual-machines/linux/mac-create-ssh-keys.md) or [Windows](../../virtual-machines/linux/ssh-from-windows.md)), use a command like hello following:</span></span>

```azurecli
az acs create -n acs-cluster -g acsrg1 -d applink789
```
<span data-ttu-id="2d632-134">Pokud veřejný klíč SSH nemáte, použijte druhý příkaz.</span><span class="sxs-lookup"><span data-stu-id="2d632-134">If you don't have an SSH public key, use this second command.</span></span> <span data-ttu-id="2d632-135">Tento příkaz s hello `--generate-ssh-keys` přepínač vytvoří za vás.</span><span class="sxs-lookup"><span data-stu-id="2d632-135">This command with hello `--generate-ssh-keys` switch creates one for you.</span></span>

```azurecli
az acs create -n acs-cluster -g acsrg1 -d applink789 --generate-ssh-keys
```

<span data-ttu-id="2d632-136">Po zadání příkazu hello, Počkejte přibližně 10 minut pro toobe hello clusteru vytvořen.</span><span class="sxs-lookup"><span data-stu-id="2d632-136">After you enter hello command, wait for about 10 minutes for hello cluster toobe created.</span></span> <span data-ttu-id="2d632-137">výstup příkazu Hello zahrnuje plně kvalifikované názvy domény (FQDN) hello hlavní a uzly agenta a SSH příkaz tooconnect toohello prvního hlavního serveru.</span><span class="sxs-lookup"><span data-stu-id="2d632-137">hello command output includes fully qualified domain names (FQDNs) of hello master and agent nodes and an SSH command tooconnect toohello first master.</span></span> <span data-ttu-id="2d632-138">Tady je zkrácený výstup:</span><span class="sxs-lookup"><span data-stu-id="2d632-138">Here is abbreviated output:</span></span>

![Obrázek s příkazem ACS create](./media/container-service-create-acs-cluster-cli/cluster-create.png)

> [!TIP]
> <span data-ttu-id="2d632-140">Hello [Kubernetes návod](../kubernetes/container-service-kubernetes-walkthrough.md) ukazuje, jak toouse `az acs create` s toocreate výchozí hodnoty Kubernetes clusteru.</span><span class="sxs-lookup"><span data-stu-id="2d632-140">hello [Kubernetes walkthrough](../kubernetes/container-service-kubernetes-walkthrough.md) shows how toouse `az acs create` with default values toocreate a Kubernetes cluster.</span></span>
>

## <a name="manage-acs-clusters"></a><span data-ttu-id="2d632-141">Správa clusterů ACS</span><span class="sxs-lookup"><span data-stu-id="2d632-141">Manage ACS clusters</span></span>

<span data-ttu-id="2d632-142">Použití další `az acs` příkazy toomanage vašeho clusteru.</span><span class="sxs-lookup"><span data-stu-id="2d632-142">Use additional `az acs` commands toomanage your cluster.</span></span> <span data-ttu-id="2d632-143">Zde je několik příkladů:</span><span class="sxs-lookup"><span data-stu-id="2d632-143">Here are some examples.</span></span>

### <a name="list-clusters-under-a-subscription"></a><span data-ttu-id="2d632-144">Výpis clusterů v rámci předplatného</span><span class="sxs-lookup"><span data-stu-id="2d632-144">List clusters under a subscription</span></span>

```azurecli
az acs list --output table
```

### <a name="list-clusters-in-a-resource-group"></a><span data-ttu-id="2d632-145">Výpis clusterů ve skupině prostředků</span><span class="sxs-lookup"><span data-stu-id="2d632-145">List clusters in a resource group</span></span>

```azurecli
az acs list -g acsrg1 --output table
```

![příkaz acs list](./media/container-service-create-acs-cluster-cli/acs-list.png)


### <a name="display-details-of-a-container-service-cluster"></a><span data-ttu-id="2d632-147">Zobrazení podrobností o clusteru Container Service</span><span class="sxs-lookup"><span data-stu-id="2d632-147">Display details of a container service cluster</span></span>

```azurecli
az acs show -g acsrg1 -n acs-cluster --output list
```

![příkaz acs show](./media/container-service-create-acs-cluster-cli/acs-show.png)


### <a name="scale-hello-cluster"></a><span data-ttu-id="2d632-149">Škálování hello clusteru</span><span class="sxs-lookup"><span data-stu-id="2d632-149">Scale hello cluster</span></span>
<span data-ttu-id="2d632-150">Je povolené horizontální navýšení i snížení kapacity agentských uzlů.</span><span class="sxs-lookup"><span data-stu-id="2d632-150">Both scaling in and scaling out of agent nodes are allowed.</span></span> <span data-ttu-id="2d632-151">Hello parametr `new-agent-count` je hello nový počet agentů v clusteru hello služby ACS.</span><span class="sxs-lookup"><span data-stu-id="2d632-151">hello parameter `new-agent-count` is hello new number of agents in hello ACS cluster.</span></span>

```azurecli
az acs scale -g acsrg1 -n acs-cluster --new-agent-count 4
```

![příkaz acs scale](./media/container-service-create-acs-cluster-cli/acs-scale.png)

## <a name="delete-a-container-service-cluster"></a><span data-ttu-id="2d632-153">Odstranění clusteru služby Container Service</span><span class="sxs-lookup"><span data-stu-id="2d632-153">Delete a container service cluster</span></span>
```azurecli
az acs delete -g acsrg1 -n acs-cluster 
```
<span data-ttu-id="2d632-154">Tento příkaz neodstraní všechny prostředky (sítě a úložiště), které jsou vytvořené během vytváření služby kontejneru hello.</span><span class="sxs-lookup"><span data-stu-id="2d632-154">This command does not delete all resources (network and storage) created while creating hello container service.</span></span> <span data-ttu-id="2d632-155">toodelete všechny prostředky snadno, je doporučeno, nasadíte každý cluster ve skupině prostředků jedinečné.</span><span class="sxs-lookup"><span data-stu-id="2d632-155">toodelete all resources easily, it is recommended you deploy each cluster in a distinct resource group.</span></span> <span data-ttu-id="2d632-156">Potom odstraňte skupinu prostředků hello při hello clusteru se už nevyžaduje.</span><span class="sxs-lookup"><span data-stu-id="2d632-156">Then, delete hello resource group when hello cluster is no longer required.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2d632-157">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2d632-157">Next steps</span></span>
<span data-ttu-id="2d632-158">Nyní když máte funkční cluster, nahlédněte do těchto dokumentů, kde naleznete podrobnosti týkající se připojení a správy:</span><span class="sxs-lookup"><span data-stu-id="2d632-158">Now that you have a functioning cluster, see these documents for connection and management details:</span></span>

* [<span data-ttu-id="2d632-159">Připojení clusteru Azure Container Service tooan</span><span class="sxs-lookup"><span data-stu-id="2d632-159">Connect tooan Azure Container Service cluster</span></span>](../container-service-connect.md)
* [<span data-ttu-id="2d632-160">Práce se službou Azure Container Service a DC/OS</span><span class="sxs-lookup"><span data-stu-id="2d632-160">Work with Azure Container Service and DC/OS</span></span>](container-service-mesos-marathon-rest.md)
* [<span data-ttu-id="2d632-161">Práce se službou Azure Container Service a nástrojem Docker Swarm</span><span class="sxs-lookup"><span data-stu-id="2d632-161">Work with Azure Container Service and Docker Swarm</span></span>](container-service-docker-swarm.md)
* [<span data-ttu-id="2d632-162">Práce s Azure Container Service a Kubernetes</span><span class="sxs-lookup"><span data-stu-id="2d632-162">Work with Azure Container Service and Kubernetes</span></span>](../kubernetes/container-service-kubernetes-walkthrough.md)