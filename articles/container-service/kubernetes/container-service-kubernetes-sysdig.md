---
title: cluster Azure Kubernetes aaaMonitor - Sysdig | Microsoft Docs
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
ms.openlocfilehash: a27813d01ee4624b9e993f6185169ad73aeec3a2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-an-azure-container-service-kubernetes-cluster-using-sysdig"></a><span data-ttu-id="c0f13-103">Monitorování clusteru Azure Container Service Kubernetes pomocí Sysdig</span><span class="sxs-lookup"><span data-stu-id="c0f13-103">Monitor an Azure Container Service Kubernetes cluster using Sysdig</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c0f13-104">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c0f13-104">Prerequisites</span></span>
<span data-ttu-id="c0f13-105">Tento návod předpokládá, že máte [vytvořit Kubernetes clusteru Azure Container Service pomocí](container-service-kubernetes-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="c0f13-105">This walkthrough assumes that you have [created a Kubernetes cluster using Azure Container Service](container-service-kubernetes-walkthrough.md).</span></span>

<span data-ttu-id="c0f13-106">Taky se předpokládá, že máte hello azure cli a kubectl nástroje nainstalované.</span><span class="sxs-lookup"><span data-stu-id="c0f13-106">It also assumes that you have hello azure cli and kubectl tools installed.</span></span>

<span data-ttu-id="c0f13-107">Pokud máte hello můžete otestovat `az` nainstalovaná, spuštěním nástroje:</span><span class="sxs-lookup"><span data-stu-id="c0f13-107">You can test if you have hello `az` tool installed by running:</span></span>

```console
$ az --version
```

<span data-ttu-id="c0f13-108">Pokud nemáte hello `az` nástroj nainstalovali, jsou k dispozici pokyny [zde](https://github.com/azure/azure-cli#installation).</span><span class="sxs-lookup"><span data-stu-id="c0f13-108">If you don't have hello `az` tool installed, there are instructions [here](https://github.com/azure/azure-cli#installation).</span></span>

<span data-ttu-id="c0f13-109">Pokud máte hello můžete otestovat `kubectl` nainstalovaná, spuštěním nástroje:</span><span class="sxs-lookup"><span data-stu-id="c0f13-109">You can test if you have hello `kubectl` tool installed by running:</span></span>

```console
$ kubectl version
```

<span data-ttu-id="c0f13-110">Pokud nemáte `kubectl` nainstalován, můžete spustit:</span><span class="sxs-lookup"><span data-stu-id="c0f13-110">If you don't have `kubectl` installed, you can run:</span></span>

```console
$ az acs kubernetes install-cli
```

## <a name="sysdig"></a><span data-ttu-id="c0f13-111">Sysdig</span><span class="sxs-lookup"><span data-stu-id="c0f13-111">Sysdig</span></span>
<span data-ttu-id="c0f13-112">Sysdig je, že externí monitorování jako služba společnosti, které můžete sledovat kontejnery v clusteru Kubernetes běžící v Azure.</span><span class="sxs-lookup"><span data-stu-id="c0f13-112">Sysdig is an external monitoring as a service company which can monitor containers in your Kubernetes cluster running in Azure.</span></span> <span data-ttu-id="c0f13-113">Použití Sysdig vyžaduje aktivní Sysdig účet.</span><span class="sxs-lookup"><span data-stu-id="c0f13-113">Using Sysdig requires an active Sysdig account.</span></span>
<span data-ttu-id="c0f13-114">Můžete si zaregistrovat účet jejich [lokality](https://app.sysdigcloud.com).</span><span class="sxs-lookup"><span data-stu-id="c0f13-114">You can sign up for an account on their [site](https://app.sysdigcloud.com).</span></span>

<span data-ttu-id="c0f13-115">Jakmile jste přihlášeni toohello Sysdig cloudu web, klikněte na své uživatelské jméno a na stránce hello byste měli vidět vaší "přístupový klíč."</span><span class="sxs-lookup"><span data-stu-id="c0f13-115">Once you're logged in toohello Sysdig cloud website, click on your user name, and on hello page you should see your "Access Key."</span></span> 

![Klíč rozhraní API služby Sysdig](./media/container-service-kubernetes-sysdig/sysdig2.png)

## <a name="installing-hello-sysdig-agents-tookubernetes"></a><span data-ttu-id="c0f13-117">Instalace tooKubernetes agenti Sysdig hello</span><span class="sxs-lookup"><span data-stu-id="c0f13-117">Installing hello Sysdig agents tooKubernetes</span></span>
<span data-ttu-id="c0f13-118">toomonitor kontejnerů, Sysdig spustí proces na každý počítač, použití Kubernetes `DaemonSet`.</span><span class="sxs-lookup"><span data-stu-id="c0f13-118">toomonitor your containers, Sysdig runs a process on each machine using a Kubernetes `DaemonSet`.</span></span>
<span data-ttu-id="c0f13-119">DaemonSets jsou objekty Kubernetes rozhraní API, které spustit jednu instanci kontejner na počítač.</span><span class="sxs-lookup"><span data-stu-id="c0f13-119">DaemonSets are Kubernetes API objects that run a single instance of a container per machine.</span></span>
<span data-ttu-id="c0f13-120">Jsou ideální pro instalaci nástroje jako hello agenta monitorování na Sysdig.</span><span class="sxs-lookup"><span data-stu-id="c0f13-120">They're perfect for installing tools like hello Sysdig's monitoring agent.</span></span>

<span data-ttu-id="c0f13-121">tooinstall hello Sysdig daemonset, byste si měli nejdřív Stáhnout [hello šablony](https://raw.githubusercontent.com/draios/sysdig-cloud-scripts/master/agent_deploy/kubernetes/sysdig-daemonset.yaml) z sysdig.</span><span class="sxs-lookup"><span data-stu-id="c0f13-121">tooinstall hello Sysdig daemonset, you should first download [hello template](https://raw.githubusercontent.com/draios/sysdig-cloud-scripts/master/agent_deploy/kubernetes/sysdig-daemonset.yaml) from sysdig.</span></span> <span data-ttu-id="c0f13-122">Uložte tento soubor jako `sysdig-daemonset.yaml`.</span><span class="sxs-lookup"><span data-stu-id="c0f13-122">Save that file as `sysdig-daemonset.yaml`.</span></span>

<span data-ttu-id="c0f13-123">Na Linuxu a OS X můžete spustit:</span><span class="sxs-lookup"><span data-stu-id="c0f13-123">On Linux and OS X you can run:</span></span>

```console
$ curl -O https://raw.githubusercontent.com/draios/sysdig-cloud-scripts/master/agent_deploy/kubernetes/sysdig-daemonset.yaml
```

<span data-ttu-id="c0f13-124">V prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="c0f13-124">In PowerShell:</span></span>

```console
$ Invoke-WebRequest -Uri https://raw.githubusercontent.com/draios/sysdig-cloud-scripts/master/agent_deploy/kubernetes/sysdig-daemonset.yaml | Select-Object -ExpandProperty Content > sysdig-daemonset.yaml
```

<span data-ttu-id="c0f13-125">Tento soubor tooinsert upravte vedle přístupový klíč, který jste získali z vašeho účtu Sysdig.</span><span class="sxs-lookup"><span data-stu-id="c0f13-125">Next edit that file tooinsert your Access Key, that you obtained from your Sysdig account.</span></span>

<span data-ttu-id="c0f13-126">Nakonec vytvořte hello DaemonSet:</span><span class="sxs-lookup"><span data-stu-id="c0f13-126">Finally, create hello DaemonSet:</span></span>

```console
$ kubectl create -f sysdig-daemonset.yaml
```

## <a name="view-your-monitoring"></a><span data-ttu-id="c0f13-127">Zobrazení monitorování</span><span class="sxs-lookup"><span data-stu-id="c0f13-127">View your monitoring</span></span>
<span data-ttu-id="c0f13-128">Jakmile nainstalovaná a spuštěná, by měl hello agenty čerpadla data back tooSysdig.</span><span class="sxs-lookup"><span data-stu-id="c0f13-128">Once installed and running, hello agents should pump data back tooSysdig.</span></span>  <span data-ttu-id="c0f13-129">Přejděte zpět toothe [řídicí panel sysdig](https://app.sysdigcloud.com) a měli byste vidět informace o kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="c0f13-129">Go back toothe [sysdig dashboard](https://app.sysdigcloud.com) and you should see information about your containers.</span></span>

<span data-ttu-id="c0f13-130">Můžete taky nainstalovat specifické Kubernetes řídicí panely prostřednictvím [Průvodce novým řídicím panelu](https://app.sysdigcloud.com/#/dashboards/new).</span><span class="sxs-lookup"><span data-stu-id="c0f13-130">You can also install Kubernetes-specific dashboards via the [new dashboard wizard](https://app.sysdigcloud.com/#/dashboards/new).</span></span>
