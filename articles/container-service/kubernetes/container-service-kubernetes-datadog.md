---
title: "Monitorování Azure Kubernetes cluster s Datadog | Microsoft Docs"
description: "Monitorování Kubernetes clusteru v Azure Container Service pomocí Datadog"
services: container-service
documentationcenter: 
author: bburns
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/09/2016
ms.author: bburns
ms.custom: mvc
ms.openlocfilehash: 40b34457447a8f80d8cdf77579750e0c42df22d0
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-an-azure-container-service-cluster-with-datadog"></a><span data-ttu-id="f8e1e-103">Monitorování clusteru Azure Container Service s DataDog</span><span class="sxs-lookup"><span data-stu-id="f8e1e-103">Monitor an Azure Container Service cluster with DataDog</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f8e1e-104">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f8e1e-104">Prerequisites</span></span>
<span data-ttu-id="f8e1e-105">Tento návod předpokládá, že máte [vytvořit Kubernetes clusteru Azure Container Service pomocí](container-service-kubernetes-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="f8e1e-105">This walkthrough assumes that you have [created a Kubernetes cluster using Azure Container Service](container-service-kubernetes-walkthrough.md).</span></span>

<span data-ttu-id="f8e1e-106">Předpokládá také, abyste měli `az` rozhraní příkazového řádku Azure a `kubectl` nástroje nainstalované.</span><span class="sxs-lookup"><span data-stu-id="f8e1e-106">It also assumes that you have the `az` Azure cli and `kubectl` tools installed.</span></span>

<span data-ttu-id="f8e1e-107">Můžete otestovat, pokud máte `az` nainstalovaná, spuštěním nástroje:</span><span class="sxs-lookup"><span data-stu-id="f8e1e-107">You can test if you have the `az` tool installed by running:</span></span>

```console
$ az --version
```

<span data-ttu-id="f8e1e-108">Pokud nemáte `az` nástroj nainstalovali, jsou k dispozici pokyny [zde](https://github.com/azure/azure-cli#installation).</span><span class="sxs-lookup"><span data-stu-id="f8e1e-108">If you don't have the `az` tool installed, there are instructions [here](https://github.com/azure/azure-cli#installation).</span></span>

<span data-ttu-id="f8e1e-109">Můžete otestovat, pokud máte `kubectl` nainstalovaná, spuštěním nástroje:</span><span class="sxs-lookup"><span data-stu-id="f8e1e-109">You can test if you have the `kubectl` tool installed by running:</span></span>

```console
$ kubectl version
```

<span data-ttu-id="f8e1e-110">Pokud nemáte `kubectl` nainstalován, můžete spustit:</span><span class="sxs-lookup"><span data-stu-id="f8e1e-110">If you don't have `kubectl` installed, you can run:</span></span>

```console
$ az acs kubernetes install-cli
```

## <a name="datadog"></a><span data-ttu-id="f8e1e-111">DataDog</span><span class="sxs-lookup"><span data-stu-id="f8e1e-111">DataDog</span></span>
<span data-ttu-id="f8e1e-112">Datadog je monitorování služba, která shromažďuje data monitorování z kontejnerů v rámci clusteru Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="f8e1e-112">Datadog is a monitoring service that gathers monitoring data from your containers within your Azure Container Service cluster.</span></span> <span data-ttu-id="f8e1e-113">Datadog má řídicí panel integrace Dockeru kde můžete zobrazit konkrétní metriky v rámci kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="f8e1e-113">Datadog has a Docker Integration Dashboard where you can see specific metrics within your containers.</span></span> <span data-ttu-id="f8e1e-114">Metriky shromážděná z kontejnerů jsou uspořádané podle využití procesoru, paměti, síťové a vstupně-výstupní operace.</span><span class="sxs-lookup"><span data-stu-id="f8e1e-114">Metrics gathered from your containers are organized by CPU, Memory, Network and I/O.</span></span> <span data-ttu-id="f8e1e-115">Datadog rozdělí metriky na kontejnery a obrázků.</span><span class="sxs-lookup"><span data-stu-id="f8e1e-115">Datadog splits metrics into containers and images.</span></span>

<span data-ttu-id="f8e1e-116">Je nutné nejprve [vytvoření účtu](https://www.datadoghq.com/lpg/)</span><span class="sxs-lookup"><span data-stu-id="f8e1e-116">You first need to [create an account](https://www.datadoghq.com/lpg/)</span></span>

## <a name="installing-the-datadog-agent-with-a-daemonset"></a><span data-ttu-id="f8e1e-117">Instalace agenta Datadog s DaemonSet</span><span class="sxs-lookup"><span data-stu-id="f8e1e-117">Installing the Datadog Agent with a DaemonSet</span></span>
<span data-ttu-id="f8e1e-118">DaemonSets Kubernetes používá ke spuštění jednu instanci kontejner na jednotlivých hostitelích v clusteru.</span><span class="sxs-lookup"><span data-stu-id="f8e1e-118">DaemonSets are used by Kubernetes to run a single instance of a container on each host in the cluster.</span></span>
<span data-ttu-id="f8e1e-119">Jsou ideální pro spuštění monitorovací agenty.</span><span class="sxs-lookup"><span data-stu-id="f8e1e-119">They're perfect for running monitoring agents.</span></span>

<span data-ttu-id="f8e1e-120">Po přihlášení do Datadog můžete provést [Datadog pokyny](https://app.datadoghq.com/account/settings#agent/kubernetes) k instalaci agentů Datadog v clusteru pomocí DaemonSet.</span><span class="sxs-lookup"><span data-stu-id="f8e1e-120">Once you have logged into Datadog, you can follow the [Datadog instructions](https://app.datadoghq.com/account/settings#agent/kubernetes) to install Datadog agents on your cluster using a DaemonSet.</span></span>

## <a name="conclusion"></a><span data-ttu-id="f8e1e-121">Závěr</span><span class="sxs-lookup"><span data-stu-id="f8e1e-121">Conclusion</span></span>
<span data-ttu-id="f8e1e-122">A to je vše!</span><span class="sxs-lookup"><span data-stu-id="f8e1e-122">That's it!</span></span> <span data-ttu-id="f8e1e-123">Jakmile agenti jsou spuštěná byste měli vidět data v konzole za pár minut.</span><span class="sxs-lookup"><span data-stu-id="f8e1e-123">Once the agents are up and running you should see data in the console in a few minutes.</span></span> <span data-ttu-id="f8e1e-124">Můžete navštívit integrované [řídicí panel kubernetes](https://app.datadoghq.com/screen/integration/kubernetes) se zobrazí souhrn clusteru.</span><span class="sxs-lookup"><span data-stu-id="f8e1e-124">You can visit the integrated [kubernetes dashboard](https://app.datadoghq.com/screen/integration/kubernetes) to see a summary of your cluster.</span></span>
