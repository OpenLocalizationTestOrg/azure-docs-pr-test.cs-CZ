---
title: "Kontejner Azure instance skupiny kontejnerů"
description: "Pochopit, jak fungují skupiny kontejnerů v Azure kontejner instancí"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 25eab41c3f0c986bcce33123f86f4c9638b77191
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="container-groups-in-azure-container-instances"></a><span data-ttu-id="142f1-103">Skupiny kontejnerů v Azure kontejner instancí</span><span class="sxs-lookup"><span data-stu-id="142f1-103">Container Groups in Azure Container Instances</span></span>

<span data-ttu-id="142f1-104">Nejvyšší úrovně prostředků v Azure kontejner instancí je skupina kontejneru.</span><span class="sxs-lookup"><span data-stu-id="142f1-104">The top-level resource in Azure Container Instances is a container group.</span></span> <span data-ttu-id="142f1-105">Tento článek popisuje, jaké jsou skupiny kontejnerů a jaké druhy scénářů umožňují.</span><span class="sxs-lookup"><span data-stu-id="142f1-105">This article describes what container groups are and what types of scenarios they enable.</span></span>

## <a name="how-a-container-group-works"></a><span data-ttu-id="142f1-106">Jak funguje skupina kontejneru</span><span class="sxs-lookup"><span data-stu-id="142f1-106">How a container group works</span></span>

<span data-ttu-id="142f1-107">Skupina kontejneru je kolekce kontejnery, které získat naplánovat na stejném hostitelském počítači a sdílet životního cyklu, místní sítě a svazky úložiště.</span><span class="sxs-lookup"><span data-stu-id="142f1-107">A container group is a collection of containers that get scheduled on the same host machine and share a lifecycle, local network, and storage volumes.</span></span> <span data-ttu-id="142f1-108">Je podobný koncept *pod* v [Kubernetes](https://kubernetes.io/docs/concepts/workloads/pods/pod/) a [DC/OS](https://dcos.io/docs/1.9/deploying-services/pods/).</span><span class="sxs-lookup"><span data-stu-id="142f1-108">It is similar to the concept of a *pod* in [Kubernetes](https://kubernetes.io/docs/concepts/workloads/pods/pod/) and [DC/OS](https://dcos.io/docs/1.9/deploying-services/pods/).</span></span>

<span data-ttu-id="142f1-109">Následující diagram ukazuje příklad skupinou kontejneru, která obsahuje více kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="142f1-109">The following diagram shows an example of a container group that includes multiple containers.</span></span>

![Příklad skupiny kontejneru][container-groups-example]

<span data-ttu-id="142f1-111">Poznámky:</span><span class="sxs-lookup"><span data-stu-id="142f1-111">Note that:</span></span>

- <span data-ttu-id="142f1-112">Skupiny je naplánováno na jeden hostitelský počítač.</span><span class="sxs-lookup"><span data-stu-id="142f1-112">The group is scheduled on a single host machine.</span></span>
- <span data-ttu-id="142f1-113">Skupiny zpřístupní jednu veřejnou IP adresu, s zveřejněné jeden port.</span><span class="sxs-lookup"><span data-stu-id="142f1-113">The group exposes a single public IP address, with one exposed port.</span></span>
- <span data-ttu-id="142f1-114">Skupiny je tvořen dvěma kontejnery.</span><span class="sxs-lookup"><span data-stu-id="142f1-114">The group is made up of two containers.</span></span> <span data-ttu-id="142f1-115">Jeden kontejner naslouchá na portu 80, při dalších sleduje na port 5000.</span><span class="sxs-lookup"><span data-stu-id="142f1-115">One container listens on port 80, while the other listens on port 5000.</span></span>
- <span data-ttu-id="142f1-116">Skupiny obsahuje dvě sdílené složky Azure jako svazek připojení zařízení a každý kontejner připojí některé sdílené složky místně.</span><span class="sxs-lookup"><span data-stu-id="142f1-116">The group includes two Azure file shares as volume mounts, and each container mounts one of the shares locally.</span></span>

### <a name="networking"></a><span data-ttu-id="142f1-117">Sítě</span><span class="sxs-lookup"><span data-stu-id="142f1-117">Networking</span></span>

<span data-ttu-id="142f1-118">Skupiny kontejnerů sdílet IP adresy a portu obor názvů na tuto IP adresu.</span><span class="sxs-lookup"><span data-stu-id="142f1-118">Container groups share an IP address and a port namespace on that IP address.</span></span> <span data-ttu-id="142f1-119">Pokud chcete povolit externí klienti dosáhli na kontejner v rámci skupiny, musí vystavit port na IP adrese a z kontejneru.</span><span class="sxs-lookup"><span data-stu-id="142f1-119">To enable external clients to reach a container within the group, you must expose the port on the IP address and from the container.</span></span> <span data-ttu-id="142f1-120">Vzhledem k tomu, že kontejnery v rámci skupiny sdílet port obor názvů, mapování portů se nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="142f1-120">Because containers within the group share a port namespace, port mapping is not supported.</span></span> <span data-ttu-id="142f1-121">Kontejnery v rámci skupiny dosáhnout vzájemně prostřednictvím místního hostitele na porty, které se mají vystavený, i když tyto porty se nezobrazí externě na IP adrese skupiny.</span><span class="sxs-lookup"><span data-stu-id="142f1-121">Containers within a group can reach each other via localhost on the ports that they have exposed, even if those ports are not exposed externally on the group's IP address.</span></span>

### <a name="storage"></a><span data-ttu-id="142f1-122">Úložiště</span><span class="sxs-lookup"><span data-stu-id="142f1-122">Storage</span></span>

<span data-ttu-id="142f1-123">Můžete zadat externí svazky připojit v rámci skupiny kontejneru.</span><span class="sxs-lookup"><span data-stu-id="142f1-123">You can specify external volumes to mount within a container group.</span></span> <span data-ttu-id="142f1-124">Můžete namapovat tyto svazky do konkrétních cest v rámci jednotlivých kontejnerů ve skupině.</span><span class="sxs-lookup"><span data-stu-id="142f1-124">You can map those volumes into specific paths within the individual containers in a group.</span></span>

## <a name="common-scenarios"></a><span data-ttu-id="142f1-125">Obvyklé scénáře</span><span class="sxs-lookup"><span data-stu-id="142f1-125">Common scenarios</span></span>

<span data-ttu-id="142f1-126">Více kontejner skupiny jsou užitečné v případech, ve které chcete rozdělit jeden úkol funkční do malý počet kontejneru Image, které mohou být dodány pomocí různými týmy a obsahují požadavky samostatné prostředků.</span><span class="sxs-lookup"><span data-stu-id="142f1-126">Multi-container groups are useful in cases where you want to divide up a single functional task into a small number of container images, which can be delivered by different teams and have separate resource requirements.</span></span> <span data-ttu-id="142f1-127">Příklad použití můžou zahrnovat:</span><span class="sxs-lookup"><span data-stu-id="142f1-127">Example usage could include:</span></span>

- <span data-ttu-id="142f1-128">Kontejner aplikace a kontejner protokolování.</span><span class="sxs-lookup"><span data-stu-id="142f1-128">An application container and a logging container.</span></span> <span data-ttu-id="142f1-129">Kontejner protokolování shromažďuje protokoly a metriky výstup v hlavní aplikaci a zapíše do dlouhodobého úložiště.</span><span class="sxs-lookup"><span data-stu-id="142f1-129">The logging container collects the logs and metrics output by the main application and writes them to long-term storage.</span></span>
- <span data-ttu-id="142f1-130">Aplikace a kontejner monitorování.</span><span class="sxs-lookup"><span data-stu-id="142f1-130">An application and a monitoring container.</span></span> <span data-ttu-id="142f1-131">Monitorování kontejneru pravidelně odešle požadavek na aplikaci zkontrolujte, zda je spuštěná a správně reagovat a vyvolá výstrahu, pokud není.</span><span class="sxs-lookup"><span data-stu-id="142f1-131">The monitoring container periodically makes a request to the application to ensure that it's running and responding correctly and raises an alert if it's not.</span></span>
- <span data-ttu-id="142f1-132">Kontejner obsluhující webové aplikace a kontejner stahování nejnovější obsah od správy zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="142f1-132">A container serving a web application and a container pulling the latest content from source control.</span></span>

## <a name="next-steps"></a><span data-ttu-id="142f1-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="142f1-133">Next steps</span></span>

<span data-ttu-id="142f1-134">Zjistěte, jak [nasazení s více kontejner skupiny](container-instances-multi-container-group.md) pomocí šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="142f1-134">Learn how to [deploy a multi-container group](container-instances-multi-container-group.md) with an Azure Resource Manager template.</span></span>

<!-- IMAGES -->

[container-groups-example]: ./media/container-instances-container-groups/container-groups-example.png