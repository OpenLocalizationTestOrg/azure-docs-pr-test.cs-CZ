---
title: aaaIntroduction tooAzure Container Service pro Kubernetes | Microsoft Docs
description: "Azure Container Service pro Kubernetes umožňuje jednoduché toodeploy a spravovat aplikace založené na kontejneru v Azure."
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
ms.openlocfilehash: bfc85a180bdf4a405c9047eb882d3eed01640dd1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooazure-container-service-for-kubernetes"></a><span data-ttu-id="4bde7-104">Úvod tooAzure Container Service pro Kubernetes</span><span class="sxs-lookup"><span data-stu-id="4bde7-104">Introduction tooAzure Container Service for Kubernetes</span></span>
<span data-ttu-id="4bde7-105">Azure Container Service pro Kubernetes umožňuje jednoduché toocreate, konfigurovat a spravovat cluster virtuálních počítačů, které jsou předkonfigurované toorun kontejnerové aplikace.</span><span class="sxs-lookup"><span data-stu-id="4bde7-105">Azure Container Service for Kubernetes makes it simple toocreate, configure, and manage a cluster of virtual machines that are preconfigured toorun containerized applications.</span></span> <span data-ttu-id="4bde7-106">To umožňuje vám toouse existujících dovedností, nebo kreslení na text velké a stále se rozrůstající komunita odborných znalostí, toodeploy a spravovat aplikace založené na kontejneru v Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="4bde7-106">This enables you toouse your existing skills, or draw upon a large and growing body of community expertise, toodeploy and manage container-based applications on Microsoft Azure.</span></span>

<span data-ttu-id="4bde7-107">Pomocí Azure Container Service můžete využít výhod hello podnikové úrovni funkce Azure, a současně se zachovává přenositelnost aplikace prostřednictvím Kubernetes a hello Docker formát obrázku.</span><span class="sxs-lookup"><span data-stu-id="4bde7-107">By using Azure Container Service, you can take advantage of hello enterprise-grade features of Azure, while still maintaining application portability through Kubernetes and hello Docker image format.</span></span>

## <a name="using-azure-container-service-for-kubernetes"></a><span data-ttu-id="4bde7-108">Použití služby Azure Container Service pro Kubernetes</span><span class="sxs-lookup"><span data-stu-id="4bde7-108">Using Azure Container Service for Kubernetes</span></span>
<span data-ttu-id="4bde7-109">Naším cílem s Azure Container Service je tooprovide kontejneru hostitelské prostředí pomocí nástroje open source a technologie, které jsou dnes důležitá u našich zákazníků.</span><span class="sxs-lookup"><span data-stu-id="4bde7-109">Our goal with Azure Container Service is tooprovide a container hosting environment by using open-source tools and technologies that are popular among our customers today.</span></span> <span data-ttu-id="4bde7-110">toothis end zveřejňujeme hello standardní koncové body Kubernetes rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="4bde7-110">toothis end, we expose hello standard Kubernetes API endpoints.</span></span> <span data-ttu-id="4bde7-111">Pomocí těchto standardních koncových bodů, můžete využít veškerý software, který je schopen rozhovoru tooa Kubernetes clusteru.</span><span class="sxs-lookup"><span data-stu-id="4bde7-111">By using these standard endpoints, you can leverage any software that is capable of talking tooa Kubernetes cluster.</span></span> <span data-ttu-id="4bde7-112">Můžete například zvolit [kubectl](https://kubernetes.io/docs/user-guide/kubectl-overview/), [helm](https://helm.sh/) nebo [draft](https://github.com/Azure/draft).</span><span class="sxs-lookup"><span data-stu-id="4bde7-112">For example, you might choose [kubectl](https://kubernetes.io/docs/user-guide/kubectl-overview/), [helm](https://helm.sh/), or [draft](https://github.com/Azure/draft).</span></span>

## <a name="creating-a-kubernetes-cluster-using-azure-container-service"></a><span data-ttu-id="4bde7-113">Vytvoření clusteru Kubernetes pomocí služby Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="4bde7-113">Creating a Kubernetes cluster using Azure Container Service</span></span>
<span data-ttu-id="4bde7-114">toobegin pomocí Azure Container Service, nasazení clusteru Azure Container Service s hello [Azure CLI 2.0](container-service-kubernetes-walkthrough.md) nebo přes portál hello (hledání hello Marketplace pro **Azure Container Service**).</span><span class="sxs-lookup"><span data-stu-id="4bde7-114">toobegin using Azure Container Service, deploy an Azure Container Service cluster with hello [Azure CLI 2.0](container-service-kubernetes-walkthrough.md) or via hello portal (search hello Marketplace for **Azure Container Service**).</span></span> <span data-ttu-id="4bde7-115">Pokud jste pro pokročilé uživatele, kteří musí větší kontrolu nad hello šablony Azure Resource Manager, můžete použít s otevřeným zdrojem hello [acs modul](https://github.com/Azure/acs-engine) toobuild projektu vlastní vlastní Kubernetes clusteru a nasadit prostřednictvím hello `az` rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="4bde7-115">If you are an advanced user who needs more control over hello Azure Resource Manager templates, you can use hello open source [acs-engine](https://github.com/Azure/acs-engine) project toobuild your own custom Kubernetes cluster and deploy it via hello `az` CLI.</span></span>

### <a name="using-kubernetes"></a><span data-ttu-id="4bde7-116">S použitím Kubernetes</span><span class="sxs-lookup"><span data-stu-id="4bde7-116">Using Kubernetes</span></span>
<span data-ttu-id="4bde7-117">Kubernetes automatizuje nasazení, škálování a správu kontejnerizovaných aplikací.</span><span class="sxs-lookup"><span data-stu-id="4bde7-117">Kubernetes automates deployment, scaling, and management of containerized applications.</span></span> <span data-ttu-id="4bde7-118">Má bohatou výbavu funkcí, například:</span><span class="sxs-lookup"><span data-stu-id="4bde7-118">It has a rich set of features including:</span></span>
* <span data-ttu-id="4bde7-119">Automatické balení do kontejnerů</span><span class="sxs-lookup"><span data-stu-id="4bde7-119">Automatic binpacking</span></span>
* <span data-ttu-id="4bde7-120">Samoopravení</span><span class="sxs-lookup"><span data-stu-id="4bde7-120">Self-healing</span></span>
* <span data-ttu-id="4bde7-121">Horizontální škálování</span><span class="sxs-lookup"><span data-stu-id="4bde7-121">Horizontal scaling</span></span>
* <span data-ttu-id="4bde7-122">Zjišťování služeb a vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="4bde7-122">Service discovery and load balancing</span></span>
* <span data-ttu-id="4bde7-123">Automatická uvádění a vracení zpět</span><span class="sxs-lookup"><span data-stu-id="4bde7-123">Automated rollouts and rollbacks</span></span>
* <span data-ttu-id="4bde7-124">Správa tajných kódů a konfigurací</span><span class="sxs-lookup"><span data-stu-id="4bde7-124">Secret and configuration management</span></span>
* <span data-ttu-id="4bde7-125">Orchestrace úložiště</span><span class="sxs-lookup"><span data-stu-id="4bde7-125">Storage orchestration</span></span>
* <span data-ttu-id="4bde7-126">Spouštění dávek</span><span class="sxs-lookup"><span data-stu-id="4bde7-126">Batch execution</span></span>

<span data-ttu-id="4bde7-127">Diagram architektury systému Kubernetes nasazeného prostřednictvím služby Azure Container Service:</span><span class="sxs-lookup"><span data-stu-id="4bde7-127">Architectural diagram of Kubernetes deployed via Azure Container Service:</span></span>

![Azure Container Service nakonfigurovat toouse Kubernetes.](media/acs-intro/kubernetes.png)

## <a name="videos"></a><span data-ttu-id="4bde7-129">Videa</span><span class="sxs-lookup"><span data-stu-id="4bde7-129">Videos</span></span>

<span data-ttu-id="4bde7-130">Podpora pro Kubernetes ve službě Azure Container Service (Azure Friday, leden 2017):</span><span class="sxs-lookup"><span data-stu-id="4bde7-130">Kubernetes Support in Azure Container Services (Azure Friday, January 2017):</span></span>

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Kubernetes-Support-in-Azure-Container-Services/player]
>
>

<span data-ttu-id="4bde7-131">Nástroje pro vývoj a nasazování aplikací v Kubernetes (Azure OpenDev, červen 2017):</span><span class="sxs-lookup"><span data-stu-id="4bde7-131">Tools for Developing and Deploying Applications on Kubernetes (Azure OpenDev, June 2017):</span></span>

> [!VIDEO https://channel9.msdn.com/Events/AzureOpenDev/June2017/Tools-for-Developing-and-Deploying-Applications-on-Kubernetes/player]
>
>

## <a name="next-steps"></a><span data-ttu-id="4bde7-132">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4bde7-132">Next steps</span></span>

<span data-ttu-id="4bde7-133">Prozkoumejte hello [rychlý start Kubernetes](container-service-kubernetes-walkthrough.md) toobegin dnes zkoumat Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="4bde7-133">Explore hello [Kubernetes Quickstart](container-service-kubernetes-walkthrough.md) toobegin exploring Azure Container Service today.</span></span>
