---
title: "Seznámení se službou Azure Container Service pro Kubernetes | Dokumentace Microsoftu"
description: "Azure Container Service pro Kubernetes zjednodušuje nasazování a správu aplikací založených na kontejnerech v Azure."
services: container-service
documentationcenter: 
author: gabrtv
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: Kubernetes, Docker, Containers, Microservices, Azure
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/21/2017
ms.author: gamonroy
ms.custom: mvc
ms.openlocfilehash: 92cdbe20e7a2974a734dfed5294c547866050290
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="introduction-to-azure-container-service-for-kubernetes"></a><span data-ttu-id="9abfc-104">Seznámení se službou Azure Container Service pro Kubernetes</span><span class="sxs-lookup"><span data-stu-id="9abfc-104">Introduction to Azure Container Service for Kubernetes</span></span>
<span data-ttu-id="9abfc-105">Azure Container Service pro Kubernetes zjednodušuje vytvoření, konfiguraci a správu clusteru virtuálních počítačů, ve kterých je předem nakonfigurované spouštění kontejnerizovaných aplikací.</span><span class="sxs-lookup"><span data-stu-id="9abfc-105">Azure Container Service for Kubernetes makes it simple to create, configure, and manage a cluster of virtual machines that are preconfigured to run containerized applications.</span></span> <span data-ttu-id="9abfc-106">Díky tomu můžete při nasazování a správě kontejnerových aplikací v Microsoft Azure využívat své stávající dovednosti nebo stavět na rozsáhlých a stále se rozšiřujících odborných znalostech komunity.</span><span class="sxs-lookup"><span data-stu-id="9abfc-106">This enables you to use your existing skills, or draw upon a large and growing body of community expertise, to deploy and manage container-based applications on Microsoft Azure.</span></span>

<span data-ttu-id="9abfc-107">Díky používání služby Azure Container Service můžete využívat výhody funkcí Azure na podnikové úrovni, a přitom zachovávat přenositelnost aplikací prostřednictvím Kubernetes a formátu image Dockeru.</span><span class="sxs-lookup"><span data-stu-id="9abfc-107">By using Azure Container Service, you can take advantage of the enterprise-grade features of Azure, while still maintaining application portability through Kubernetes and the Docker image format.</span></span>

## <a name="using-azure-container-service-for-kubernetes"></a><span data-ttu-id="9abfc-108">Použití služby Azure Container Service pro Kubernetes</span><span class="sxs-lookup"><span data-stu-id="9abfc-108">Using Azure Container Service for Kubernetes</span></span>
<span data-ttu-id="9abfc-109">Naším cílem u služby Azure Container Service je poskytnout hostitelské prostředí pro kontejnery díky používání nástrojů a technologií open source, které jsou dnes mezi našimi zákazníky oblíbené.</span><span class="sxs-lookup"><span data-stu-id="9abfc-109">Our goal with Azure Container Service is to provide a container hosting environment by using open-source tools and technologies that are popular among our customers today.</span></span> <span data-ttu-id="9abfc-110">Za tímto účelem zveřejňujeme standardní koncové body rozhraní Kubernetes API.</span><span class="sxs-lookup"><span data-stu-id="9abfc-110">To this end, we expose the standard Kubernetes API endpoints.</span></span> <span data-ttu-id="9abfc-111">S použitím těchto standardních koncových bodů můžete využívat veškerý software, který dokáže komunikovat s clusterem Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="9abfc-111">By using these standard endpoints, you can leverage any software that is capable of talking to a Kubernetes cluster.</span></span> <span data-ttu-id="9abfc-112">Můžete například zvolit [kubectl](https://kubernetes.io/docs/user-guide/kubectl-overview/), [helm](https://helm.sh/) nebo [draft](https://github.com/Azure/draft).</span><span class="sxs-lookup"><span data-stu-id="9abfc-112">For example, you might choose [kubectl](https://kubernetes.io/docs/user-guide/kubectl-overview/), [helm](https://helm.sh/), or [draft](https://github.com/Azure/draft).</span></span>

## <a name="creating-a-kubernetes-cluster-using-azure-container-service"></a><span data-ttu-id="9abfc-113">Vytvoření clusteru Kubernetes pomocí služby Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="9abfc-113">Creating a Kubernetes cluster using Azure Container Service</span></span>
<span data-ttu-id="9abfc-114">Pokud chcete začít používat službu Azure Container Service, nasaďte cluster Azure Container Service pomocí [Azure CLI 2.0](container-service-kubernetes-walkthrough.md) nebo přes portál (na webu Azure Marketplace vyhledejte **Azure Container Service**).</span><span class="sxs-lookup"><span data-stu-id="9abfc-114">To begin using Azure Container Service, deploy an Azure Container Service cluster with the [Azure CLI 2.0](container-service-kubernetes-walkthrough.md) or via the portal (search the Marketplace for **Azure Container Service**).</span></span> <span data-ttu-id="9abfc-115">Pokud jste pokročilý uživatel a potřebujete větší kontrolu nad šablonami Azure Resource Manageru, můžete pomocí open source projektu [acs-engine](https://github.com/Azure/acs-engine) sestavit vlastní cluster Kubernetes a nasadit ho přes rozhraní příkazového řádku `az`.</span><span class="sxs-lookup"><span data-stu-id="9abfc-115">If you are an advanced user who needs more control over the Azure Resource Manager templates, you can use the open source [acs-engine](https://github.com/Azure/acs-engine) project to build your own custom Kubernetes cluster and deploy it via the `az` CLI.</span></span>

### <a name="using-kubernetes"></a><span data-ttu-id="9abfc-116">S použitím Kubernetes</span><span class="sxs-lookup"><span data-stu-id="9abfc-116">Using Kubernetes</span></span>
<span data-ttu-id="9abfc-117">Kubernetes automatizuje nasazení, škálování a správu kontejnerizovaných aplikací.</span><span class="sxs-lookup"><span data-stu-id="9abfc-117">Kubernetes automates deployment, scaling, and management of containerized applications.</span></span> <span data-ttu-id="9abfc-118">Má bohatou výbavu funkcí, například:</span><span class="sxs-lookup"><span data-stu-id="9abfc-118">It has a rich set of features including:</span></span>
* <span data-ttu-id="9abfc-119">Automatické balení do kontejnerů</span><span class="sxs-lookup"><span data-stu-id="9abfc-119">Automatic binpacking</span></span>
* <span data-ttu-id="9abfc-120">Samoopravení</span><span class="sxs-lookup"><span data-stu-id="9abfc-120">Self-healing</span></span>
* <span data-ttu-id="9abfc-121">Horizontální škálování</span><span class="sxs-lookup"><span data-stu-id="9abfc-121">Horizontal scaling</span></span>
* <span data-ttu-id="9abfc-122">Zjišťování služeb a vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="9abfc-122">Service discovery and load balancing</span></span>
* <span data-ttu-id="9abfc-123">Automatická uvádění a vracení zpět</span><span class="sxs-lookup"><span data-stu-id="9abfc-123">Automated rollouts and rollbacks</span></span>
* <span data-ttu-id="9abfc-124">Správa tajných kódů a konfigurací</span><span class="sxs-lookup"><span data-stu-id="9abfc-124">Secret and configuration management</span></span>
* <span data-ttu-id="9abfc-125">Orchestrace úložiště</span><span class="sxs-lookup"><span data-stu-id="9abfc-125">Storage orchestration</span></span>
* <span data-ttu-id="9abfc-126">Spouštění dávek</span><span class="sxs-lookup"><span data-stu-id="9abfc-126">Batch execution</span></span>

<span data-ttu-id="9abfc-127">Diagram architektury systému Kubernetes nasazeného prostřednictvím služby Azure Container Service:</span><span class="sxs-lookup"><span data-stu-id="9abfc-127">Architectural diagram of Kubernetes deployed via Azure Container Service:</span></span>

![Služba Azure Container Service nakonfigurovaná pro používání Kubernetes.](media/acs-intro/kubernetes.png)

## <a name="videos"></a><span data-ttu-id="9abfc-129">Videa</span><span class="sxs-lookup"><span data-stu-id="9abfc-129">Videos</span></span>

<span data-ttu-id="9abfc-130">Podpora pro Kubernetes ve službě Azure Container Service (Azure Friday, leden 2017):</span><span class="sxs-lookup"><span data-stu-id="9abfc-130">Kubernetes Support in Azure Container Services (Azure Friday, January 2017):</span></span>

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Kubernetes-Support-in-Azure-Container-Services/player]
>
>

<span data-ttu-id="9abfc-131">Nástroje pro vývoj a nasazování aplikací v Kubernetes (Azure OpenDev, červen 2017):</span><span class="sxs-lookup"><span data-stu-id="9abfc-131">Tools for Developing and Deploying Applications on Kubernetes (Azure OpenDev, June 2017):</span></span>

> [!VIDEO https://channel9.msdn.com/Events/AzureOpenDev/June2017/Tools-for-Developing-and-Deploying-Applications-on-Kubernetes/player]
>
>

## <a name="next-steps"></a><span data-ttu-id="9abfc-132">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9abfc-132">Next steps</span></span>

<span data-ttu-id="9abfc-133">Přečtěte si [Rychlý start s Kubernetes](container-service-kubernetes-walkthrough.md) a začněte prozkoumávat službu Azure Container Service ještě dnes.</span><span class="sxs-lookup"><span data-stu-id="9abfc-133">Explore the [Kubernetes Quickstart](container-service-kubernetes-walkthrough.md) to begin exploring Azure Container Service today.</span></span>