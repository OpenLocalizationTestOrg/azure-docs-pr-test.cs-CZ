---
title: "Cluster Azure Kubernetes monitorování - Sysdig | Microsoft Docs"
description: "Monitorování Kubernetes clusteru v Azure Container Service pomocí Sysdig"
services: container-service
documentationcenter: 
author: bburns
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/09/2016
ms.author: bburns
ms.custom: mvc
ms.openlocfilehash: afe22b84015526f901111238e36baaa94694ccbf
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-an-azure-container-service-kubernetes-cluster-using-sysdig"></a><span data-ttu-id="0d61b-103">Monitorování clusteru Azure Container Service Kubernetes pomocí Sysdig</span><span class="sxs-lookup"><span data-stu-id="0d61b-103">Monitor an Azure Container Service Kubernetes cluster using Sysdig</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0d61b-104">Požadavky</span><span class="sxs-lookup"><span data-stu-id="0d61b-104">Prerequisites</span></span>
<span data-ttu-id="0d61b-105">Tento návod předpokládá, že máte [vytvořit Kubernetes clusteru Azure Container Service pomocí](container-service-kubernetes-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="0d61b-105">This walkthrough assumes that you have [created a Kubernetes cluster using Azure Container Service](container-service-kubernetes-walkthrough.md).</span></span>

<span data-ttu-id="0d61b-106">Předpokládá také, že máte azure cli a kubectl nástroje nainstalované.</span><span class="sxs-lookup"><span data-stu-id="0d61b-106">It also assumes that you have the azure cli and kubectl tools installed.</span></span>

<span data-ttu-id="0d61b-107">Můžete otestovat, pokud máte `az` nainstalovaná, spuštěním nástroje:</span><span class="sxs-lookup"><span data-stu-id="0d61b-107">You can test if you have the `az` tool installed by running:</span></span>

```console
$ az --version
```

<span data-ttu-id="0d61b-108">Pokud nemáte `az` nástroj nainstalovali, jsou k dispozici pokyny [zde](https://github.com/azure/azure-cli#installation).</span><span class="sxs-lookup"><span data-stu-id="0d61b-108">If you don't have the `az` tool installed, there are instructions [here](https://github.com/azure/azure-cli#installation).</span></span>

<span data-ttu-id="0d61b-109">Můžete otestovat, pokud máte `kubectl` nainstalovaná, spuštěním nástroje:</span><span class="sxs-lookup"><span data-stu-id="0d61b-109">You can test if you have the `kubectl` tool installed by running:</span></span>

```console
$ kubectl version
```

<span data-ttu-id="0d61b-110">Pokud nemáte `kubectl` nainstalován, můžete spustit:</span><span class="sxs-lookup"><span data-stu-id="0d61b-110">If you don't have `kubectl` installed, you can run:</span></span>

```console
$ az acs kubernetes install-cli
```

## <a name="sysdig"></a><span data-ttu-id="0d61b-111">Sysdig</span><span class="sxs-lookup"><span data-stu-id="0d61b-111">Sysdig</span></span>
<span data-ttu-id="0d61b-112">Sysdig je, že externí monitorování jako služba společnosti, které můžete sledovat kontejnery v clusteru Kubernetes běžící v Azure.</span><span class="sxs-lookup"><span data-stu-id="0d61b-112">Sysdig is an external monitoring as a service company which can monitor containers in your Kubernetes cluster running in Azure.</span></span> <span data-ttu-id="0d61b-113">Použití Sysdig vyžaduje aktivní Sysdig účet.</span><span class="sxs-lookup"><span data-stu-id="0d61b-113">Using Sysdig requires an active Sysdig account.</span></span>
<span data-ttu-id="0d61b-114">Můžete si zaregistrovat účet jejich [lokality](https://app.sysdigcloud.com).</span><span class="sxs-lookup"><span data-stu-id="0d61b-114">You can sign up for an account on their [site](https://app.sysdigcloud.com).</span></span>

<span data-ttu-id="0d61b-115">Po přihlášení na web cloudu Sysdig klikněte na uživatelské jméno. Zobrazí se stránka, na které byste měli najít svůj „přístupový klíč“.</span><span class="sxs-lookup"><span data-stu-id="0d61b-115">Once you're logged in to the Sysdig cloud website, click on your user name, and on the page you should see your "Access Key."</span></span> 

![Klíč rozhraní API služby Sysdig](./media/container-service-kubernetes-sysdig/sysdig2.png)

## <a name="installing-the-sysdig-agents-to-kubernetes"></a><span data-ttu-id="0d61b-117">Instalace agentů Sysdig k Kubernetes</span><span class="sxs-lookup"><span data-stu-id="0d61b-117">Installing the Sysdig agents to Kubernetes</span></span>
<span data-ttu-id="0d61b-118">Ke sledování kontejnerů, Sysdig spustí nějaký proces na každém počítači pomocí Kubernetes `DaemonSet`.</span><span class="sxs-lookup"><span data-stu-id="0d61b-118">To monitor your containers, Sysdig runs a process on each machine using a Kubernetes `DaemonSet`.</span></span>
<span data-ttu-id="0d61b-119">DaemonSets jsou objekty Kubernetes rozhraní API, které spustit jednu instanci kontejner na počítač.</span><span class="sxs-lookup"><span data-stu-id="0d61b-119">DaemonSets are Kubernetes API objects that run a single instance of a container per machine.</span></span>
<span data-ttu-id="0d61b-120">Jsou ideální pro instalaci nástroje, například Sysdig agent monitorování.</span><span class="sxs-lookup"><span data-stu-id="0d61b-120">They're perfect for installing tools like the Sysdig's monitoring agent.</span></span>

<span data-ttu-id="0d61b-121">Pokud chcete nainstalovat Sysdig daemonset, musí nejdřív Stáhnout [šablony](https://raw.githubusercontent.com/draios/sysdig-cloud-scripts/master/agent_deploy/kubernetes/sysdig-daemonset.yaml) z sysdig.</span><span class="sxs-lookup"><span data-stu-id="0d61b-121">To install the Sysdig daemonset, you should first download [the template](https://raw.githubusercontent.com/draios/sysdig-cloud-scripts/master/agent_deploy/kubernetes/sysdig-daemonset.yaml) from sysdig.</span></span> <span data-ttu-id="0d61b-122">Uložte tento soubor jako `sysdig-daemonset.yaml`.</span><span class="sxs-lookup"><span data-stu-id="0d61b-122">Save that file as `sysdig-daemonset.yaml`.</span></span>

<span data-ttu-id="0d61b-123">Na Linuxu a OS X můžete spustit:</span><span class="sxs-lookup"><span data-stu-id="0d61b-123">On Linux and OS X you can run:</span></span>

```console
$ curl -O https://raw.githubusercontent.com/draios/sysdig-cloud-scripts/master/agent_deploy/kubernetes/sysdig-daemonset.yaml
```

<span data-ttu-id="0d61b-124">V prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="0d61b-124">In PowerShell:</span></span>

```console
$ Invoke-WebRequest -Uri https://raw.githubusercontent.com/draios/sysdig-cloud-scripts/master/agent_deploy/kubernetes/sysdig-daemonset.yaml | Select-Object -ExpandProperty Content > sysdig-daemonset.yaml
```

<span data-ttu-id="0d61b-125">Tento soubor vložit přístupový klíč, který jste získali z vašeho účtu Sysdig dále upravte.</span><span class="sxs-lookup"><span data-stu-id="0d61b-125">Next edit that file to insert your Access Key, that you obtained from your Sysdig account.</span></span>

<span data-ttu-id="0d61b-126">Nakonec vytvořte DaemonSet:</span><span class="sxs-lookup"><span data-stu-id="0d61b-126">Finally, create the DaemonSet:</span></span>

```console
$ kubectl create -f sysdig-daemonset.yaml
```

## <a name="view-your-monitoring"></a><span data-ttu-id="0d61b-127">Zobrazení monitorování</span><span class="sxs-lookup"><span data-stu-id="0d61b-127">View your monitoring</span></span>
<span data-ttu-id="0d61b-128">Jakmile nainstalovaná a spuštěná, by měl agenty čerpadla data zpět do Sysdig.</span><span class="sxs-lookup"><span data-stu-id="0d61b-128">Once installed and running, the agents should pump data back to Sysdig.</span></span>  <span data-ttu-id="0d61b-129">Přejděte zpět [sysdig řídicí panel](https://app.sysdigcloud.com) a měli byste vidět informace o kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="0d61b-129">Go back to the [sysdig dashboard](https://app.sysdigcloud.com) and you should see information about your containers.</span></span>

<span data-ttu-id="0d61b-130">Můžete taky nainstalovat specifické Kubernetes řídicí panely prostřednictvím [Průvodce novým řídicím panelu](https://app.sysdigcloud.com/#/dashboards/new).</span><span class="sxs-lookup"><span data-stu-id="0d61b-130">You can also install Kubernetes-specific dashboards via the [new dashboard wizard](https://app.sysdigcloud.com/#/dashboards/new).</span></span>
