---
title: aaaMonitor Azure Kubernetes cluster s Datadog | Microsoft Docs
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
ms.openlocfilehash: bccd8b59a048e0f791172fcfc4eeba6370dafcc0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-an-azure-container-service-cluster-with-datadog"></a><span data-ttu-id="976fe-103">Monitorování clusteru Azure Container Service s DataDog</span><span class="sxs-lookup"><span data-stu-id="976fe-103">Monitor an Azure Container Service cluster with DataDog</span></span>

## <a name="prerequisites"></a><span data-ttu-id="976fe-104">Požadavky</span><span class="sxs-lookup"><span data-stu-id="976fe-104">Prerequisites</span></span>
<span data-ttu-id="976fe-105">Tento návod předpokládá, že máte [vytvořit Kubernetes clusteru Azure Container Service pomocí](container-service-kubernetes-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="976fe-105">This walkthrough assumes that you have [created a Kubernetes cluster using Azure Container Service](container-service-kubernetes-walkthrough.md).</span></span>

<span data-ttu-id="976fe-106">Také předpokládá, že máte hello `az` rozhraní příkazového řádku Azure a `kubectl` nástroje nainstalované.</span><span class="sxs-lookup"><span data-stu-id="976fe-106">It also assumes that you have hello `az` Azure cli and `kubectl` tools installed.</span></span>

<span data-ttu-id="976fe-107">Pokud máte hello můžete otestovat `az` nainstalovaná, spuštěním nástroje:</span><span class="sxs-lookup"><span data-stu-id="976fe-107">You can test if you have hello `az` tool installed by running:</span></span>

```console
$ az --version
```

<span data-ttu-id="976fe-108">Pokud nemáte hello `az` nástroj nainstalovali, jsou k dispozici pokyny [zde](https://github.com/azure/azure-cli#installation).</span><span class="sxs-lookup"><span data-stu-id="976fe-108">If you don't have hello `az` tool installed, there are instructions [here](https://github.com/azure/azure-cli#installation).</span></span>

<span data-ttu-id="976fe-109">Pokud máte hello můžete otestovat `kubectl` nainstalovaná, spuštěním nástroje:</span><span class="sxs-lookup"><span data-stu-id="976fe-109">You can test if you have hello `kubectl` tool installed by running:</span></span>

```console
$ kubectl version
```

<span data-ttu-id="976fe-110">Pokud nemáte `kubectl` nainstalován, můžete spustit:</span><span class="sxs-lookup"><span data-stu-id="976fe-110">If you don't have `kubectl` installed, you can run:</span></span>

```console
$ az acs kubernetes install-cli
```

## <a name="datadog"></a><span data-ttu-id="976fe-111">DataDog</span><span class="sxs-lookup"><span data-stu-id="976fe-111">DataDog</span></span>
<span data-ttu-id="976fe-112">Datadog je monitorování služba, která shromažďuje data monitorování z kontejnerů v rámci clusteru Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="976fe-112">Datadog is a monitoring service that gathers monitoring data from your containers within your Azure Container Service cluster.</span></span> <span data-ttu-id="976fe-113">Datadog má řídicí panel integrace Dockeru kde můžete zobrazit konkrétní metriky v rámci kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="976fe-113">Datadog has a Docker Integration Dashboard where you can see specific metrics within your containers.</span></span> <span data-ttu-id="976fe-114">Metriky shromážděná z kontejnerů jsou uspořádané podle využití procesoru, paměti, síťové a vstupně-výstupní operace.</span><span class="sxs-lookup"><span data-stu-id="976fe-114">Metrics gathered from your containers are organized by CPU, Memory, Network and I/O.</span></span> <span data-ttu-id="976fe-115">Datadog rozdělí metriky na kontejnery a obrázků.</span><span class="sxs-lookup"><span data-stu-id="976fe-115">Datadog splits metrics into containers and images.</span></span>

<span data-ttu-id="976fe-116">Je nutné nejprve příliš[vytvoření účtu](https://www.datadoghq.com/lpg/)</span><span class="sxs-lookup"><span data-stu-id="976fe-116">You first need too[create an account](https://www.datadoghq.com/lpg/)</span></span>

## <a name="installing-hello-datadog-agent-with-a-daemonset"></a><span data-ttu-id="976fe-117">Instalace s DaemonSet hello Datadog agenta</span><span class="sxs-lookup"><span data-stu-id="976fe-117">Installing hello Datadog Agent with a DaemonSet</span></span>
<span data-ttu-id="976fe-118">DaemonSets používají Kubernetes toorun jednu instanci kontejner na jednotlivých hostitelích v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="976fe-118">DaemonSets are used by Kubernetes toorun a single instance of a container on each host in hello cluster.</span></span>
<span data-ttu-id="976fe-119">Jsou ideální pro spuštění monitorovací agenty.</span><span class="sxs-lookup"><span data-stu-id="976fe-119">They're perfect for running monitoring agents.</span></span>

<span data-ttu-id="976fe-120">Po přihlášení do Datadog můžete postupovat podle hello [Datadog pokyny](https://app.datadoghq.com/account/settings#agent/kubernetes) tooinstall Datadog agentů v clusteru pomocí DaemonSet.</span><span class="sxs-lookup"><span data-stu-id="976fe-120">Once you have logged into Datadog, you can follow hello [Datadog instructions](https://app.datadoghq.com/account/settings#agent/kubernetes) tooinstall Datadog agents on your cluster using a DaemonSet.</span></span>

## <a name="conclusion"></a><span data-ttu-id="976fe-121">Závěr</span><span class="sxs-lookup"><span data-stu-id="976fe-121">Conclusion</span></span>
<span data-ttu-id="976fe-122">A to je vše!</span><span class="sxs-lookup"><span data-stu-id="976fe-122">That's it!</span></span> <span data-ttu-id="976fe-123">Jakmile jsou agenti hello a systémem jste měli vidět data v konzole hello za pár minut.</span><span class="sxs-lookup"><span data-stu-id="976fe-123">Once hello agents are up and running you should see data in hello console in a few minutes.</span></span> <span data-ttu-id="976fe-124">Můžete taky navštívit hello integrované [řídicí panel kubernetes](https://app.datadoghq.com/screen/integration/kubernetes) toosee souhrn clusteru.</span><span class="sxs-lookup"><span data-stu-id="976fe-124">You can visit hello integrated [kubernetes dashboard](https://app.datadoghq.com/screen/integration/kubernetes) toosee a summary of your cluster.</span></span>
