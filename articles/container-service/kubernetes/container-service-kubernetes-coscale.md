---
title: aaaMonitor Azure Kubernetes cluster s CoScale | Microsoft Docs
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
ms.openlocfilehash: f835e82d2be3afe1d85070bd0bf69649cc6dd2ff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-an-azure-container-service-kubernetes-cluster-with-coscale"></a><span data-ttu-id="5b923-103">Monitorování clusteru Azure Container Service Kubernetes s CoScale</span><span class="sxs-lookup"><span data-stu-id="5b923-103">Monitor an Azure Container Service Kubernetes cluster with CoScale</span></span>

<span data-ttu-id="5b923-104">V tomto článku jsme ukazují, jak toodeploy hello [CoScale](https://www.coscale.com/) toomonitor agenta všechny uzly a kontejnery v vaší Kubernetes cluster v Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="5b923-104">In this article, we show you how toodeploy hello [CoScale](https://www.coscale.com/) agent toomonitor all nodes and containers in your Kubernetes cluster in Azure Container Service.</span></span> <span data-ttu-id="5b923-105">Účet s CoScale musíte pro tuto konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="5b923-105">You need an account with CoScale for this configuration.</span></span> 


## <a name="about-coscale"></a><span data-ttu-id="5b923-106">O CoScale</span><span class="sxs-lookup"><span data-stu-id="5b923-106">About CoScale</span></span> 

<span data-ttu-id="5b923-107">CoScale je monitorování platforma, která shromažďuje metriky a události z všechny kontejnery v několik platforem orchestration.</span><span class="sxs-lookup"><span data-stu-id="5b923-107">CoScale is a monitoring platform that gathers metrics and events from all containers in several orchestration platforms.</span></span> <span data-ttu-id="5b923-108">CoScale nabízí úplné zásobníku monitorování pro Kubernetes prostředí.</span><span class="sxs-lookup"><span data-stu-id="5b923-108">CoScale offers full-stack monitoring for Kubernetes environments.</span></span> <span data-ttu-id="5b923-109">Poskytuje vizualizace a analýzy pro všechny vrstvy v zásobníku hello: hello operačního systému, Kubernetes, Docker a aplikacemi spuštěnými ve kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="5b923-109">It provides visualizations and analytics for all layers in hello stack: hello OS, Kubernetes, Docker, and applications running inside your containers.</span></span> <span data-ttu-id="5b923-110">CoScale nabízí několik předdefinovaných monitorování řídicí panely a má integrovanou anomálií detekce tooallow operátory a rychlé vývojáři toofind infrastruktury a aplikace potíže.</span><span class="sxs-lookup"><span data-stu-id="5b923-110">CoScale offers several built-in monitoring dashboards, and it has built-in anomaly detection tooallow operators and developers toofind infrastructure and application issues fast.</span></span>

![CoScale uživatelského rozhraní](./media/container-service-kubernetes-coscale/coscale.png)

<span data-ttu-id="5b923-112">Jak je znázorněno v tomto článku, můžete nainstalovat agenty na clusteru toorun Kubernetes CoScale jako řešení SaaS.</span><span class="sxs-lookup"><span data-stu-id="5b923-112">As shown in this article, you can install agents on a Kubernetes cluster toorun CoScale as a SaaS solution.</span></span> <span data-ttu-id="5b923-113">Pokud chcete tookeep vaše data na místě, CoScale je také k dispozici pro místní instalaci.</span><span class="sxs-lookup"><span data-stu-id="5b923-113">If you want tookeep your data on-site, CoScale is also available for on-premises installation.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="5b923-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5b923-114">Prerequisites</span></span>

<span data-ttu-id="5b923-115">Je nutné nejprve příliš[vytvořit účet CoScale](https://www.coscale.com/free-trial).</span><span class="sxs-lookup"><span data-stu-id="5b923-115">You first need too[create a CoScale account](https://www.coscale.com/free-trial).</span></span>

<span data-ttu-id="5b923-116">Tento návod předpokládá, že máte [vytvořit Kubernetes clusteru Azure Container Service pomocí](container-service-kubernetes-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="5b923-116">This walkthrough assumes that you have [created a Kubernetes cluster using Azure Container Service](container-service-kubernetes-walkthrough.md).</span></span>

<span data-ttu-id="5b923-117">Také předpokládá, že máte hello `az` rozhraní příkazového řádku Azure a `kubectl` nástroje nainstalované.</span><span class="sxs-lookup"><span data-stu-id="5b923-117">It also assumes that you have hello `az` Azure CLI and `kubectl` tools installed.</span></span>

<span data-ttu-id="5b923-118">Pokud máte hello můžete otestovat `az` nainstalovaná, spuštěním nástroje:</span><span class="sxs-lookup"><span data-stu-id="5b923-118">You can test if you have hello `az` tool installed by running:</span></span>

```azurecli
az --version
```

<span data-ttu-id="5b923-119">Pokud nemáte hello `az` nástroj nainstalovali, jsou k dispozici pokyny [zde](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="5b923-119">If you don't have hello `az` tool installed, there are instructions [here](/cli/azure/install-azure-cli).</span></span>

<span data-ttu-id="5b923-120">Pokud máte hello můžete otestovat `kubectl` nainstalovaná, spuštěním nástroje:</span><span class="sxs-lookup"><span data-stu-id="5b923-120">You can test if you have hello `kubectl` tool installed by running:</span></span>

```bash
kubectl version
```

<span data-ttu-id="5b923-121">Pokud nemáte `kubectl` nainstalován, můžete spustit:</span><span class="sxs-lookup"><span data-stu-id="5b923-121">If you don't have `kubectl` installed, you can run:</span></span>

```azurecli
az acs kubernetes install-cli
```

## <a name="installing-hello-coscale-agent-with-a-daemonset"></a><span data-ttu-id="5b923-122">Instalace agenta CoScale hello s DaemonSet</span><span class="sxs-lookup"><span data-stu-id="5b923-122">Installing hello CoScale agent with a DaemonSet</span></span>
<span data-ttu-id="5b923-123">[DaemonSets](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) jsou Kubernetes toorun používá jednu instanci kontejner na jednotlivých hostitelích v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="5b923-123">[DaemonSets](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) are used by Kubernetes toorun a single instance of a container on each host in hello cluster.</span></span>
<span data-ttu-id="5b923-124">Jsou ideální pro spuštění monitorovací agenty například hello CoScale agenta.</span><span class="sxs-lookup"><span data-stu-id="5b923-124">They're perfect for running monitoring agents such as hello CoScale agent.</span></span>

<span data-ttu-id="5b923-125">Po přihlášení tooCoScale přejděte toohello [agent stránka](https://app.coscale.com/) tooinstall CoScale agentů v clusteru pomocí DaemonSet.</span><span class="sxs-lookup"><span data-stu-id="5b923-125">After you log in tooCoScale, go toohello [agent page](https://app.coscale.com/) tooinstall CoScale agents on your cluster using a DaemonSet.</span></span> <span data-ttu-id="5b923-126">Hello uživatelského rozhraní CoScale poskytuje toocreate kroky s průvodcem konfigurace agenta a spuštění monitorování vaší Kubernetes clusterů.</span><span class="sxs-lookup"><span data-stu-id="5b923-126">hello CoScale UI provides guided configuration steps toocreate an agent and start monitoring your complete Kubernetes cluster.</span></span>

![Konfigurace agenta coScale](./media/container-service-kubernetes-coscale/installation.png)

<span data-ttu-id="5b923-128">agent hello toostart na hello clusteru, spusťte příkaz hello zadat:</span><span class="sxs-lookup"><span data-stu-id="5b923-128">toostart hello agent on hello cluster, run hello supplied command:</span></span>

![Spuštění agenta CoScale hello](./media/container-service-kubernetes-coscale/agent_script.png)

<span data-ttu-id="5b923-130">A to je vše!</span><span class="sxs-lookup"><span data-stu-id="5b923-130">That's it!</span></span> <span data-ttu-id="5b923-131">Jakmile hello agenti jsou spuštěná, měli byste vidět data v konzole hello za pár minut.</span><span class="sxs-lookup"><span data-stu-id="5b923-131">Once hello agents are up and running, you should see data in hello console in a few minutes.</span></span> <span data-ttu-id="5b923-132">Navštivte hello [agent stránka](https://app.coscale.com/) toosee souhrn cluster, provést další kroky konfigurace a v tématu řídicí panely, jako je například hello **Kubernetes clusteru přehled**.</span><span class="sxs-lookup"><span data-stu-id="5b923-132">Visit hello [agent page](https://app.coscale.com/) toosee a summary of your cluster, perform additional configuration steps, and see dashboards such as hello **Kubernetes cluster overview**.</span></span>

![Přehled Kubernetes clusteru](./media/container-service-kubernetes-coscale/dashboard_clusteroverview.png)

<span data-ttu-id="5b923-134">Hello CoScale agenta je automaticky nasadit na nové počítače v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="5b923-134">hello CoScale agent is automatically deployed on new machines in hello cluster.</span></span> <span data-ttu-id="5b923-135">aktualizace agenta Hello automaticky, když je vydána nová verze.</span><span class="sxs-lookup"><span data-stu-id="5b923-135">hello agent updates automatically when a new version is released.</span></span>


## <a name="next-steps"></a><span data-ttu-id="5b923-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5b923-136">Next steps</span></span>

<span data-ttu-id="5b923-137">V tématu hello [CoScale dokumentace](http://docs.coscale.com/) a [blog](https://www.coscale.com/blog) Další informace o CoScale sledování řešení.</span><span class="sxs-lookup"><span data-stu-id="5b923-137">See hello [CoScale documentation](http://docs.coscale.com/) and [blog](https://www.coscale.com/blog) for more more information about CoScale monitoring solutions.</span></span> 

