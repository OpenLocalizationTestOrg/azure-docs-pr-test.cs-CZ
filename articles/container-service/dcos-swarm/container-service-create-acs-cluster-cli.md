---
title: "Nasazení clusteru kontejneru Dockeru – Azure CLI | Dokumentace Microsoftu"
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
ms.openlocfilehash: ecac5c255735b588ebb512b183e8a8bbbdcc905f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="deploy-a-docker-container-hosting-solution-using-the-azure-cli-20"></a><span data-ttu-id="20b1c-103">Nasazení řešení hostování kontejnerů Dockeru pomocí Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="20b1c-103">Deploy a Docker container hosting solution using the Azure CLI 2.0</span></span>

<span data-ttu-id="20b1c-104">Pomocí příkazů `az acs` v Azure CLI 2.0 můžete vytvořit a spravovat clustery ve službě Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="20b1c-104">Use the `az acs` commands in the Azure CLI 2.0 to create and manage clusters in Azure Container Service.</span></span> <span data-ttu-id="20b1c-105">Cluster Azure Container Service můžete také nasadit pomocí webu [Azure Portal](container-service-deployment.md) nebo rozhraní API služby Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="20b1c-105">You can also deploy an Azure Container Service cluster by using the [Azure portal](container-service-deployment.md) or the Azure Container Service APIs.</span></span>

<span data-ttu-id="20b1c-106">Nápovědu k příkazům `az acs` získáte předáním parametru `-h` příslušnému příkazu.</span><span class="sxs-lookup"><span data-stu-id="20b1c-106">For help on `az acs` commands, pass the `-h` parameter to any command.</span></span> <span data-ttu-id="20b1c-107">Například: `az acs create -h`.</span><span class="sxs-lookup"><span data-stu-id="20b1c-107">For example: `az acs create -h`.</span></span>



## <a name="prerequisites"></a><span data-ttu-id="20b1c-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="20b1c-108">Prerequisites</span></span>
<span data-ttu-id="20b1c-109">K vytvoření clusteru Azure Container Service pomocí Azure CLI 2.0 musíte mít:</span><span class="sxs-lookup"><span data-stu-id="20b1c-109">To create an Azure Container Service cluster using the Azure CLI 2.0, you must:</span></span>
* <span data-ttu-id="20b1c-110">účet Azure ([získejte bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/)),</span><span class="sxs-lookup"><span data-stu-id="20b1c-110">have an Azure account ([get a free trial](https://azure.microsoft.com/pricing/free-trial/))</span></span>
* <span data-ttu-id="20b1c-111">nainstalované a nastavené [Azure CLI 2.0](/cli/azure/install-az-cli2)</span><span class="sxs-lookup"><span data-stu-id="20b1c-111">have installed and set up the [Azure CLI 2.0](/cli/azure/install-az-cli2)</span></span>

## <a name="get-started"></a><span data-ttu-id="20b1c-112">Začínáme</span><span class="sxs-lookup"><span data-stu-id="20b1c-112">Get started</span></span> 
### <a name="log-in-to-your-account"></a><span data-ttu-id="20b1c-113">Přihlášení k účtu</span><span class="sxs-lookup"><span data-stu-id="20b1c-113">Log in to your account</span></span>
```azurecli
az login 
```

<span data-ttu-id="20b1c-114">Postupujte podle zobrazených výzev a interaktivně se přihlaste.</span><span class="sxs-lookup"><span data-stu-id="20b1c-114">Follow the prompts to log in interactively.</span></span> <span data-ttu-id="20b1c-115">Další způsoby přihlášení najdete v tématu [Začínáme s Azure CLI 2.0](/cli/azure/get-started-with-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="20b1c-115">For other methods to log in, see [Get started with Azure CLI 2.0](/cli/azure/get-started-with-az-cli2).</span></span>

### <a name="set-your-azure-subscription"></a><span data-ttu-id="20b1c-116">Nastavení předplatného Azure</span><span class="sxs-lookup"><span data-stu-id="20b1c-116">Set your Azure subscription</span></span>

<span data-ttu-id="20b1c-117">Pokud máte více než jedno předplatné Azure, nastavte výchozí předplatné.</span><span class="sxs-lookup"><span data-stu-id="20b1c-117">If you have more than one Azure subscription, set the default subscription.</span></span> <span data-ttu-id="20b1c-118">Například:</span><span class="sxs-lookup"><span data-stu-id="20b1c-118">For example:</span></span>

```
az account set --subscription "f66xxxxx-xxxx-xxxx-xxx-zgxxxx33cha5"
```


### <a name="create-a-resource-group"></a><span data-ttu-id="20b1c-119">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="20b1c-119">Create a resource group</span></span>
<span data-ttu-id="20b1c-120">Doporučujeme vytvořit skupinu prostředků pro každý cluster.</span><span class="sxs-lookup"><span data-stu-id="20b1c-120">We recommend that you create a resource group for every cluster.</span></span> <span data-ttu-id="20b1c-121">Zadejte oblast Azure, ve které je Azure Container Service [k dispozici](https://azure.microsoft.com/en-us/regions/services/).</span><span class="sxs-lookup"><span data-stu-id="20b1c-121">Specify an Azure region in which Azure Container Service is [available](https://azure.microsoft.com/en-us/regions/services/).</span></span> <span data-ttu-id="20b1c-122">Například:</span><span class="sxs-lookup"><span data-stu-id="20b1c-122">For example:</span></span>

```azurecli
az group create -n acsrg1 -l "westus"
```
<span data-ttu-id="20b1c-123">Výstup je podobný tomuto:</span><span class="sxs-lookup"><span data-stu-id="20b1c-123">Output is similar to the following:</span></span>

![Vytvoření skupiny prostředků](./media/container-service-create-acs-cluster-cli/rg-create.png)


## <a name="create-an-azure-container-service-cluster"></a><span data-ttu-id="20b1c-125">Vytvoření clusteru Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="20b1c-125">Create an Azure Container Service cluster</span></span>

<span data-ttu-id="20b1c-126">K vytvoření clusteru použijte příkaz `az acs create`.</span><span class="sxs-lookup"><span data-stu-id="20b1c-126">To create a cluster, use `az acs create`.</span></span>
<span data-ttu-id="20b1c-127">Název clusteru a název skupiny prostředků vytvořené v předchozím kroku jsou povinné parametry.</span><span class="sxs-lookup"><span data-stu-id="20b1c-127">A name for the cluster and the name of the resource group created in the previous step are mandatory parameters.</span></span> 

<span data-ttu-id="20b1c-128">Ostatní vstupy jsou nastavené na výchozí hodnoty (viz následující obrazovku), pokud nejsou přepsané pomocí příslušných přepínačů.</span><span class="sxs-lookup"><span data-stu-id="20b1c-128">Other inputs are set to default values (see the following screen) unless overwritten using their respective switches.</span></span> <span data-ttu-id="20b1c-129">Například orchestrátor je standardně nastaven na DC/OS.</span><span class="sxs-lookup"><span data-stu-id="20b1c-129">For example, the orchestrator is set by default to DC/OS.</span></span> <span data-ttu-id="20b1c-130">A pokud nezadáte předponu názvu DNS, vytvoří se na základě názvu clusteru.</span><span class="sxs-lookup"><span data-stu-id="20b1c-130">And if you don't specify one, a DNS name prefix is created based on the cluster name.</span></span>

![použití příkazu az acs create](./media/container-service-create-acs-cluster-cli/create-help.png)


### <a name="quick-acs-create-using-defaults"></a><span data-ttu-id="20b1c-132">Rychlý příkaz `acs create` s využitím výchozích hodnot</span><span class="sxs-lookup"><span data-stu-id="20b1c-132">Quick `acs create` using defaults</span></span>
<span data-ttu-id="20b1c-133">Pokud máte soubor s veřejným klíčem SSH RSA `id_rsa.pub` ve výchozím umístění (nebo jste jej vytvořili pro [OS X a Linux](../../virtual-machines/linux/mac-create-ssh-keys.md) nebo [Windows](../../virtual-machines/linux/ssh-from-windows.md)), použijte příkaz podobný tomuto:</span><span class="sxs-lookup"><span data-stu-id="20b1c-133">If you have an SSH RSA public key file `id_rsa.pub` in the default location (or created one for [OS X and Linux](../../virtual-machines/linux/mac-create-ssh-keys.md) or [Windows](../../virtual-machines/linux/ssh-from-windows.md)), use a command like the following:</span></span>

```azurecli
az acs create -n acs-cluster -g acsrg1 -d applink789
```
<span data-ttu-id="20b1c-134">Pokud veřejný klíč SSH nemáte, použijte druhý příkaz.</span><span class="sxs-lookup"><span data-stu-id="20b1c-134">If you don't have an SSH public key, use this second command.</span></span> <span data-ttu-id="20b1c-135">Tento příkaz ho pro vás pomocí přepínače `--generate-ssh-keys` vytvoří.</span><span class="sxs-lookup"><span data-stu-id="20b1c-135">This command with the `--generate-ssh-keys` switch creates one for you.</span></span>

```azurecli
az acs create -n acs-cluster -g acsrg1 -d applink789 --generate-ssh-keys
```

<span data-ttu-id="20b1c-136">Po zadání příkazu počkejte asi 10 minut, než se cluster vytvoří.</span><span class="sxs-lookup"><span data-stu-id="20b1c-136">After you enter the command, wait for about 10 minutes for the cluster to be created.</span></span> <span data-ttu-id="20b1c-137">Výstup příkazu zahrnuje plně kvalifikované názvy domén hlavních i agentských uzlů a příkaz SSH pro připojení k prvnímu hlavnímu uzlu.</span><span class="sxs-lookup"><span data-stu-id="20b1c-137">The command output includes fully qualified domain names (FQDNs) of the master and agent nodes and an SSH command to connect to the first master.</span></span> <span data-ttu-id="20b1c-138">Tady je zkrácený výstup:</span><span class="sxs-lookup"><span data-stu-id="20b1c-138">Here is abbreviated output:</span></span>

![Obrázek s příkazem ACS create](./media/container-service-create-acs-cluster-cli/cluster-create.png)

> [!TIP]
> <span data-ttu-id="20b1c-140">[Názorný průvodce pro Kubernetes](../kubernetes/container-service-kubernetes-walkthrough.md) ukazuje, jak pomocí příkazu `az acs create` s použitím výchozích hodnot vytvořit cluster Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="20b1c-140">The [Kubernetes walkthrough](../kubernetes/container-service-kubernetes-walkthrough.md) shows how to use `az acs create` with default values to create a Kubernetes cluster.</span></span>
>

## <a name="manage-acs-clusters"></a><span data-ttu-id="20b1c-141">Správa clusterů ACS</span><span class="sxs-lookup"><span data-stu-id="20b1c-141">Manage ACS clusters</span></span>

<span data-ttu-id="20b1c-142">Cluster můžete spravovat pomocí dalších příkazů `az acs`.</span><span class="sxs-lookup"><span data-stu-id="20b1c-142">Use additional `az acs` commands to manage your cluster.</span></span> <span data-ttu-id="20b1c-143">Zde je několik příkladů:</span><span class="sxs-lookup"><span data-stu-id="20b1c-143">Here are some examples.</span></span>

### <a name="list-clusters-under-a-subscription"></a><span data-ttu-id="20b1c-144">Výpis clusterů v rámci předplatného</span><span class="sxs-lookup"><span data-stu-id="20b1c-144">List clusters under a subscription</span></span>

```azurecli
az acs list --output table
```

### <a name="list-clusters-in-a-resource-group"></a><span data-ttu-id="20b1c-145">Výpis clusterů ve skupině prostředků</span><span class="sxs-lookup"><span data-stu-id="20b1c-145">List clusters in a resource group</span></span>

```azurecli
az acs list -g acsrg1 --output table
```

![příkaz acs list](./media/container-service-create-acs-cluster-cli/acs-list.png)


### <a name="display-details-of-a-container-service-cluster"></a><span data-ttu-id="20b1c-147">Zobrazení podrobností o clusteru Container Service</span><span class="sxs-lookup"><span data-stu-id="20b1c-147">Display details of a container service cluster</span></span>

```azurecli
az acs show -g acsrg1 -n acs-cluster --output list
```

![příkaz acs show](./media/container-service-create-acs-cluster-cli/acs-show.png)


### <a name="scale-the-cluster"></a><span data-ttu-id="20b1c-149">Škálování clusteru</span><span class="sxs-lookup"><span data-stu-id="20b1c-149">Scale the cluster</span></span>
<span data-ttu-id="20b1c-150">Je povolené horizontální navýšení i snížení kapacity agentských uzlů.</span><span class="sxs-lookup"><span data-stu-id="20b1c-150">Both scaling in and scaling out of agent nodes are allowed.</span></span> <span data-ttu-id="20b1c-151">Parametr `new-agent-count` určuje nový počet agentů v clusteru ACS.</span><span class="sxs-lookup"><span data-stu-id="20b1c-151">The parameter `new-agent-count` is the new number of agents in the ACS cluster.</span></span>

```azurecli
az acs scale -g acsrg1 -n acs-cluster --new-agent-count 4
```

![příkaz acs scale](./media/container-service-create-acs-cluster-cli/acs-scale.png)

## <a name="delete-a-container-service-cluster"></a><span data-ttu-id="20b1c-153">Odstranění clusteru služby Container Service</span><span class="sxs-lookup"><span data-stu-id="20b1c-153">Delete a container service cluster</span></span>
```azurecli
az acs delete -g acsrg1 -n acs-cluster 
```
<span data-ttu-id="20b1c-154">Tento příkaz neodstraní všechny prostředky (sítě a úložiště), které byly vytvořené během vytváření služby Container Service.</span><span class="sxs-lookup"><span data-stu-id="20b1c-154">This command does not delete all resources (network and storage) created while creating the container service.</span></span> <span data-ttu-id="20b1c-155">Pokud chcete jednoduše odstranit všechny prostředky, doporučujeme, abyste každý cluster nasadili do jiné skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="20b1c-155">To delete all resources easily, it is recommended you deploy each cluster in a distinct resource group.</span></span> <span data-ttu-id="20b1c-156">Když už cluster nebudete potřebovat, odstraňte příslušnou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="20b1c-156">Then, delete the resource group when the cluster is no longer required.</span></span>

## <a name="next-steps"></a><span data-ttu-id="20b1c-157">Další kroky</span><span class="sxs-lookup"><span data-stu-id="20b1c-157">Next steps</span></span>
<span data-ttu-id="20b1c-158">Nyní když máte funkční cluster, nahlédněte do těchto dokumentů, kde naleznete podrobnosti týkající se připojení a správy:</span><span class="sxs-lookup"><span data-stu-id="20b1c-158">Now that you have a functioning cluster, see these documents for connection and management details:</span></span>

* [<span data-ttu-id="20b1c-159">Připojení ke clusteru Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="20b1c-159">Connect to an Azure Container Service cluster</span></span>](../container-service-connect.md)
* [<span data-ttu-id="20b1c-160">Práce se službou Azure Container Service a DC/OS</span><span class="sxs-lookup"><span data-stu-id="20b1c-160">Work with Azure Container Service and DC/OS</span></span>](container-service-mesos-marathon-rest.md)
* [<span data-ttu-id="20b1c-161">Práce se službou Azure Container Service a nástrojem Docker Swarm</span><span class="sxs-lookup"><span data-stu-id="20b1c-161">Work with Azure Container Service and Docker Swarm</span></span>](container-service-docker-swarm.md)
* [<span data-ttu-id="20b1c-162">Práce s Azure Container Service a Kubernetes</span><span class="sxs-lookup"><span data-stu-id="20b1c-162">Work with Azure Container Service and Kubernetes</span></span>](../kubernetes/container-service-kubernetes-walkthrough.md)