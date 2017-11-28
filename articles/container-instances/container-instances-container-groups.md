---
title: "aaaAzure skupiny kontejner instancí kontejnerů"
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
ms.openlocfilehash: 2b0e784609979045c8f77d7b6d0987ec3fee714c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="container-groups-in-azure-container-instances"></a><span data-ttu-id="a45c9-103">Skupiny kontejnerů v Azure kontejner instancí</span><span class="sxs-lookup"><span data-stu-id="a45c9-103">Container Groups in Azure Container Instances</span></span>

<span data-ttu-id="a45c9-104">Hello nejvyšší úrovně prostředků v Azure kontejner instancí je skupina kontejneru.</span><span class="sxs-lookup"><span data-stu-id="a45c9-104">hello top-level resource in Azure Container Instances is a container group.</span></span> <span data-ttu-id="a45c9-105">Tento článek popisuje, jaké jsou skupiny kontejnerů a jaké druhy scénářů umožňují.</span><span class="sxs-lookup"><span data-stu-id="a45c9-105">This article describes what container groups are and what types of scenarios they enable.</span></span>

## <a name="how-a-container-group-works"></a><span data-ttu-id="a45c9-106">Jak funguje skupina kontejneru</span><span class="sxs-lookup"><span data-stu-id="a45c9-106">How a container group works</span></span>

<span data-ttu-id="a45c9-107">Skupina kontejneru je kolekce kontejnery, které získat naplánovat na hello stejné hostitele počítače a sdílené složky životního cyklu, místní sítí a svazky úložiště.</span><span class="sxs-lookup"><span data-stu-id="a45c9-107">A container group is a collection of containers that get scheduled on hello same host machine and share a lifecycle, local network, and storage volumes.</span></span> <span data-ttu-id="a45c9-108">Je podobný koncept toohello *pod* v [Kubernetes](https://kubernetes.io/docs/concepts/workloads/pods/pod/) a [DC/OS](https://dcos.io/docs/1.9/deploying-services/pods/).</span><span class="sxs-lookup"><span data-stu-id="a45c9-108">It is similar toohello concept of a *pod* in [Kubernetes](https://kubernetes.io/docs/concepts/workloads/pods/pod/) and [DC/OS](https://dcos.io/docs/1.9/deploying-services/pods/).</span></span>

<span data-ttu-id="a45c9-109">Hello následující diagram ukazuje příklad skupinou kontejneru, která obsahuje více kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="a45c9-109">hello following diagram shows an example of a container group that includes multiple containers.</span></span>

![Příklad skupiny kontejneru][container-groups-example]

<span data-ttu-id="a45c9-111">Poznámky:</span><span class="sxs-lookup"><span data-stu-id="a45c9-111">Note that:</span></span>

- <span data-ttu-id="a45c9-112">skupiny Hello je naplánováno na jeden hostitelský počítač.</span><span class="sxs-lookup"><span data-stu-id="a45c9-112">hello group is scheduled on a single host machine.</span></span>
- <span data-ttu-id="a45c9-113">skupiny Hello zpřístupní jednu veřejnou IP adresu, s zveřejněné jeden port.</span><span class="sxs-lookup"><span data-stu-id="a45c9-113">hello group exposes a single public IP address, with one exposed port.</span></span>
- <span data-ttu-id="a45c9-114">skupiny Hello je tvořen dvěma kontejnery.</span><span class="sxs-lookup"><span data-stu-id="a45c9-114">hello group is made up of two containers.</span></span> <span data-ttu-id="a45c9-115">Jednoho kontejneru typu naslouchá na portu 80, zatímco jiné hello naslouchá na portu 5000.</span><span class="sxs-lookup"><span data-stu-id="a45c9-115">One container listens on port 80, while hello other listens on port 5000.</span></span>
- <span data-ttu-id="a45c9-116">Hello skupina obsahuje dvě sdílené složky Azure jako svazek připojení zařízení a každý kontejner připojí jeden hello místně sdílených složek.</span><span class="sxs-lookup"><span data-stu-id="a45c9-116">hello group includes two Azure file shares as volume mounts, and each container mounts one of hello shares locally.</span></span>

### <a name="networking"></a><span data-ttu-id="a45c9-117">Sítě</span><span class="sxs-lookup"><span data-stu-id="a45c9-117">Networking</span></span>

<span data-ttu-id="a45c9-118">Skupiny kontejnerů sdílet IP adresy a portu obor názvů na tuto IP adresu.</span><span class="sxs-lookup"><span data-stu-id="a45c9-118">Container groups share an IP address and a port namespace on that IP address.</span></span> <span data-ttu-id="a45c9-119">Externí klienti tooreach tooenable kontejner v rámci skupiny hello, musí vystavit hello port hello IP adresy a z kontejneru hello.</span><span class="sxs-lookup"><span data-stu-id="a45c9-119">tooenable external clients tooreach a container within hello group, you must expose hello port on hello IP address and from hello container.</span></span> <span data-ttu-id="a45c9-120">Vzhledem k tomu, že kontejnery v rámci skupiny hello sdílet port obor názvů, mapování portů se nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="a45c9-120">Because containers within hello group share a port namespace, port mapping is not supported.</span></span> <span data-ttu-id="a45c9-121">Kontejnery v rámci skupiny dosáhnout vzájemně prostřednictvím místního hostitele na hello porty, které se mají vystavený, i když tyto porty se nezobrazí externě na IP adrese hello skupiny.</span><span class="sxs-lookup"><span data-stu-id="a45c9-121">Containers within a group can reach each other via localhost on hello ports that they have exposed, even if those ports are not exposed externally on hello group's IP address.</span></span>

### <a name="storage"></a><span data-ttu-id="a45c9-122">Úložiště</span><span class="sxs-lookup"><span data-stu-id="a45c9-122">Storage</span></span>

<span data-ttu-id="a45c9-123">Můžete zadat toomount externí svazky v rámci skupiny kontejneru.</span><span class="sxs-lookup"><span data-stu-id="a45c9-123">You can specify external volumes toomount within a container group.</span></span> <span data-ttu-id="a45c9-124">Můžete namapovat tyto svazky do konkrétních cest v rámci jednotlivých kontejnerů hello ve skupině.</span><span class="sxs-lookup"><span data-stu-id="a45c9-124">You can map those volumes into specific paths within hello individual containers in a group.</span></span>

## <a name="common-scenarios"></a><span data-ttu-id="a45c9-125">Obvyklé scénáře</span><span class="sxs-lookup"><span data-stu-id="a45c9-125">Common scenarios</span></span>

<span data-ttu-id="a45c9-126">Více kontejner skupiny jsou užitečné v případech, místo, kam chcete toodivide až jeden úkol funkční do malý počet kontejneru Image, které mohou být dodány pomocí různými týmy a obsahují požadavky samostatné prostředků.</span><span class="sxs-lookup"><span data-stu-id="a45c9-126">Multi-container groups are useful in cases where you want toodivide up a single functional task into a small number of container images, which can be delivered by different teams and have separate resource requirements.</span></span> <span data-ttu-id="a45c9-127">Příklad použití můžou zahrnovat:</span><span class="sxs-lookup"><span data-stu-id="a45c9-127">Example usage could include:</span></span>

- <span data-ttu-id="a45c9-128">Kontejner aplikace a kontejner protokolování.</span><span class="sxs-lookup"><span data-stu-id="a45c9-128">An application container and a logging container.</span></span> <span data-ttu-id="a45c9-129">kontejner protokolování Hello shromažďuje protokoly hello a metriky výstup hello hlavní aplikace a zapíše toolong termín úložiště.</span><span class="sxs-lookup"><span data-stu-id="a45c9-129">hello logging container collects hello logs and metrics output by hello main application and writes them toolong-term storage.</span></span>
- <span data-ttu-id="a45c9-130">Aplikace a kontejner monitorování.</span><span class="sxs-lookup"><span data-stu-id="a45c9-130">An application and a monitoring container.</span></span> <span data-ttu-id="a45c9-131">Hello monitorování kontejneru pravidelně díky tooensure aplikace požadavek toohello, že je spuštěná a správně reagovat a vyvolá výstrahu, pokud není.</span><span class="sxs-lookup"><span data-stu-id="a45c9-131">hello monitoring container periodically makes a request toohello application tooensure that it's running and responding correctly and raises an alert if it's not.</span></span>
- <span data-ttu-id="a45c9-132">Kontejner obsluhující webové aplikace a kontejner stahování hello nejnovější obsah od správy zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="a45c9-132">A container serving a web application and a container pulling hello latest content from source control.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a45c9-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a45c9-133">Next steps</span></span>

<span data-ttu-id="a45c9-134">Zjistěte, jak příliš[nasazení s více kontejner skupiny](container-instances-multi-container-group.md) pomocí šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="a45c9-134">Learn how too[deploy a multi-container group](container-instances-multi-container-group.md) with an Azure Resource Manager template.</span></span>

<!-- IMAGES -->

[container-groups-example]: ./media/container-instances-container-groups/container-groups-example.png