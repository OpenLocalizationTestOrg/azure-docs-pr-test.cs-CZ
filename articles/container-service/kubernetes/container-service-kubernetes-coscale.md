---
title: "Monitorování clusteru služby Azure Kubernetes s CoScale | Microsoft Docs"
description: "Monitorování Kubernetes cluster Azure Container Service pomocí CoScale"
services: container-service
documentationcenter: 
author: fryckbos
manager: 
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/22/2017
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: f894191baced710fc0f5a8c8692df98033341a48
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-an-azure-container-service-kubernetes-cluster-with-coscale"></a><span data-ttu-id="06130-103">Monitorování clusteru Azure Container Service Kubernetes s CoScale</span><span class="sxs-lookup"><span data-stu-id="06130-103">Monitor an Azure Container Service Kubernetes cluster with CoScale</span></span>

<span data-ttu-id="06130-104">V tomto článku jsme ukazují, jak nasadit [CoScale](https://www.coscale.com/) agenta ke sledování všech uzlů a kontejnery v clusteru Kubernetes v Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="06130-104">In this article, we show you how to deploy the [CoScale](https://www.coscale.com/) agent to monitor all nodes and containers in your Kubernetes cluster in Azure Container Service.</span></span> <span data-ttu-id="06130-105">Účet s CoScale musíte pro tuto konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="06130-105">You need an account with CoScale for this configuration.</span></span> 


## <a name="about-coscale"></a><span data-ttu-id="06130-106">O CoScale</span><span class="sxs-lookup"><span data-stu-id="06130-106">About CoScale</span></span> 

<span data-ttu-id="06130-107">CoScale je monitorování platforma, která shromažďuje metriky a události z všechny kontejnery v několik platforem orchestration.</span><span class="sxs-lookup"><span data-stu-id="06130-107">CoScale is a monitoring platform that gathers metrics and events from all containers in several orchestration platforms.</span></span> <span data-ttu-id="06130-108">CoScale nabízí úplné zásobníku monitorování pro Kubernetes prostředí.</span><span class="sxs-lookup"><span data-stu-id="06130-108">CoScale offers full-stack monitoring for Kubernetes environments.</span></span> <span data-ttu-id="06130-109">Poskytuje vizualizace a analýzy pro všechny vrstvy v zásobníku: operačního systému, Kubernetes, Docker a aplikacemi spuštěnými ve kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="06130-109">It provides visualizations and analytics for all layers in the stack: the OS, Kubernetes, Docker, and applications running inside your containers.</span></span> <span data-ttu-id="06130-110">CoScale nabízí několik předdefinovaných monitorování řídicí panely a má detekce anomálií předdefinované umožňující operátory a vývojářům rychlého hledání problémy infrastruktury a aplikace.</span><span class="sxs-lookup"><span data-stu-id="06130-110">CoScale offers several built-in monitoring dashboards, and it has built-in anomaly detection to allow operators and developers to find infrastructure and application issues fast.</span></span>

![CoScale uživatelského rozhraní](./media/container-service-kubernetes-coscale/coscale.png)

<span data-ttu-id="06130-112">Jak je znázorněno v tomto článku, můžete nainstalovat agenty na clusteru s podporou Kubernetes spuštění CoScale jako řešení SaaS.</span><span class="sxs-lookup"><span data-stu-id="06130-112">As shown in this article, you can install agents on a Kubernetes cluster to run CoScale as a SaaS solution.</span></span> <span data-ttu-id="06130-113">Pokud chcete, aby vaše data na místě, CoScale je také k dispozici pro místní instalaci.</span><span class="sxs-lookup"><span data-stu-id="06130-113">If you want to keep your data on-site, CoScale is also available for on-premises installation.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="06130-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="06130-114">Prerequisites</span></span>

<span data-ttu-id="06130-115">Je nutné nejprve [vytvořit účet CoScale](https://www.coscale.com/free-trial).</span><span class="sxs-lookup"><span data-stu-id="06130-115">You first need to [create a CoScale account](https://www.coscale.com/free-trial).</span></span>

<span data-ttu-id="06130-116">Tento návod předpokládá, že máte [vytvořit Kubernetes clusteru Azure Container Service pomocí](container-service-kubernetes-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="06130-116">This walkthrough assumes that you have [created a Kubernetes cluster using Azure Container Service](container-service-kubernetes-walkthrough.md).</span></span>

<span data-ttu-id="06130-117">Předpokládá také, abyste měli `az` rozhraní příkazového řádku Azure a `kubectl` nástroje nainstalované.</span><span class="sxs-lookup"><span data-stu-id="06130-117">It also assumes that you have the `az` Azure CLI and `kubectl` tools installed.</span></span>

<span data-ttu-id="06130-118">Můžete otestovat, pokud máte `az` nainstalovaná, spuštěním nástroje:</span><span class="sxs-lookup"><span data-stu-id="06130-118">You can test if you have the `az` tool installed by running:</span></span>

```azurecli
az --version
```

<span data-ttu-id="06130-119">Pokud nemáte `az` nástroj nainstalovali, jsou k dispozici pokyny [zde](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="06130-119">If you don't have the `az` tool installed, there are instructions [here](/cli/azure/install-azure-cli).</span></span>

<span data-ttu-id="06130-120">Můžete otestovat, pokud máte `kubectl` nainstalovaná, spuštěním nástroje:</span><span class="sxs-lookup"><span data-stu-id="06130-120">You can test if you have the `kubectl` tool installed by running:</span></span>

```bash
kubectl version
```

<span data-ttu-id="06130-121">Pokud nemáte `kubectl` nainstalován, můžete spustit:</span><span class="sxs-lookup"><span data-stu-id="06130-121">If you don't have `kubectl` installed, you can run:</span></span>

```azurecli
az acs kubernetes install-cli
```

## <a name="installing-the-coscale-agent-with-a-daemonset"></a><span data-ttu-id="06130-122">Instalace agenta CoScale s DaemonSet</span><span class="sxs-lookup"><span data-stu-id="06130-122">Installing the CoScale agent with a DaemonSet</span></span>
<span data-ttu-id="06130-123">[DaemonSets](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) používají Kubernetes spustit jednu instanci kontejner na jednotlivých hostitelích v clusteru.</span><span class="sxs-lookup"><span data-stu-id="06130-123">[DaemonSets](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) are used by Kubernetes to run a single instance of a container on each host in the cluster.</span></span>
<span data-ttu-id="06130-124">Jsou ideální pro spuštění monitorování agentů, jako je například CoScale agenta.</span><span class="sxs-lookup"><span data-stu-id="06130-124">They're perfect for running monitoring agents such as the CoScale agent.</span></span>

<span data-ttu-id="06130-125">Po přihlášení do CoScale, přejděte na [agent stránka](https://app.coscale.com/) k instalaci agentů CoScale v clusteru pomocí DaemonSet.</span><span class="sxs-lookup"><span data-stu-id="06130-125">After you log in to CoScale, go to the [agent page](https://app.coscale.com/) to install CoScale agents on your cluster using a DaemonSet.</span></span> <span data-ttu-id="06130-126">Rozhraní CoScale poskytuje informací konfigurace kroky k vytvoření agenta a začít monitorovat kompletní Kubernetes clusteru.</span><span class="sxs-lookup"><span data-stu-id="06130-126">The CoScale UI provides guided configuration steps to create an agent and start monitoring your complete Kubernetes cluster.</span></span>

![Konfigurace agenta coScale](./media/container-service-kubernetes-coscale/installation.png)

<span data-ttu-id="06130-128">Spuštění agenta v clusteru, spusťte zadaný příkaz:</span><span class="sxs-lookup"><span data-stu-id="06130-128">To start the agent on the cluster, run the supplied command:</span></span>

![Spuštění agenta CoScale](./media/container-service-kubernetes-coscale/agent_script.png)

<span data-ttu-id="06130-130">A to je vše!</span><span class="sxs-lookup"><span data-stu-id="06130-130">That's it!</span></span> <span data-ttu-id="06130-131">Jakmile agenti jsou spuštěná, měli byste vidět data v konzole za pár minut.</span><span class="sxs-lookup"><span data-stu-id="06130-131">Once the agents are up and running, you should see data in the console in a few minutes.</span></span> <span data-ttu-id="06130-132">Navštivte [agent stránka](https://app.coscale.com/) souhrn clusteru najdete provést další kroky konfigurace a zobrazit řídicí panely, jako **Kubernetes clusteru přehled**.</span><span class="sxs-lookup"><span data-stu-id="06130-132">Visit the [agent page](https://app.coscale.com/) to see a summary of your cluster, perform additional configuration steps, and see dashboards such as the **Kubernetes cluster overview**.</span></span>

![Přehled Kubernetes clusteru](./media/container-service-kubernetes-coscale/dashboard_clusteroverview.png)

<span data-ttu-id="06130-134">CoScale agenta je automaticky nasadit na nové počítače v clusteru.</span><span class="sxs-lookup"><span data-stu-id="06130-134">The CoScale agent is automatically deployed on new machines in the cluster.</span></span> <span data-ttu-id="06130-135">Aktualizace agenta automaticky, když je vydána nová verze.</span><span class="sxs-lookup"><span data-stu-id="06130-135">The agent updates automatically when a new version is released.</span></span>


## <a name="next-steps"></a><span data-ttu-id="06130-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="06130-136">Next steps</span></span>

<span data-ttu-id="06130-137">Najdete v článku [CoScale dokumentace](http://docs.coscale.com/) a [blog](https://www.coscale.com/blog) Další informace o CoScale sledování řešení.</span><span class="sxs-lookup"><span data-stu-id="06130-137">See the [CoScale documentation](http://docs.coscale.com/) and [blog](https://www.coscale.com/blog) for more more information about CoScale monitoring solutions.</span></span> 

