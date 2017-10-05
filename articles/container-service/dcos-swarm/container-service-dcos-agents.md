---
title: Fondy agenta DC/OS pro Azure Container Service | Microsoft Docs
description: "Jak fungují veřejné a privátní agenta fondy pomocí clusteru služby Azure Container Service DC/OS"
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, kontejnery, mikroslužby, Mesos, Azure"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/04/2017
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: da4a196b1a73c78dfff7d8310edcc349b8d10665
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="dcos-agent-pools-for-azure-container-service"></a><span data-ttu-id="bea15-104">Fondy agenta DC/OS pro Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="bea15-104">DC/OS agent pools for Azure Container Service</span></span>
<span data-ttu-id="bea15-105">Clustery DC/OS v Azure Container Service obsahovat agenta uzly ve dvou fondů, veřejné fondu a privátní fondu.</span><span class="sxs-lookup"><span data-stu-id="bea15-105">DC/OS clusters in Azure Container Service contain agent nodes in two pools, a public pool and a private pool.</span></span> <span data-ttu-id="bea15-106">Aplikace lze nasadit do buď fondu, které mají vliv na usnadnění přístupu mezi počítači v kontejneru služby.</span><span class="sxs-lookup"><span data-stu-id="bea15-106">An application can be deployed to either pool, affecting accessibility between machines in your container service.</span></span> <span data-ttu-id="bea15-107">Na počítačích, můžete přístup k Internetu (veřejných) nebo zachovány interní (soukromý).</span><span class="sxs-lookup"><span data-stu-id="bea15-107">The machines can be exposed to the internet (public) or kept internal (private).</span></span> <span data-ttu-id="bea15-108">Tento článek poskytuje stručný přehled proto nejsou veřejné a privátní fondy.</span><span class="sxs-lookup"><span data-stu-id="bea15-108">This article gives a brief overview of why there are public and private pools.</span></span>


* <span data-ttu-id="bea15-109">**Privátní agenti**: uzly privátní agenta spustit přes síť s směrovat.</span><span class="sxs-lookup"><span data-stu-id="bea15-109">**Private agents**: Private agent nodes run through a non-routable network.</span></span> <span data-ttu-id="bea15-110">Tato síť je přístupné pouze ze zóny správce nebo prostřednictvím hraniční směrovač veřejné zóny.</span><span class="sxs-lookup"><span data-stu-id="bea15-110">This network is only accessible from the admin zone or through the public zone edge router.</span></span> <span data-ttu-id="bea15-111">Ve výchozím nastavení spustí DC/OS aplikace na uzlech privátní agenta.</span><span class="sxs-lookup"><span data-stu-id="bea15-111">By default, DC/OS launches apps on private agent nodes.</span></span> 

* <span data-ttu-id="bea15-112">**Veřejné agenty**: uzly veřejného agenta spustit DC/OS aplikace a služby přes síť s veřejně přístupná.</span><span class="sxs-lookup"><span data-stu-id="bea15-112">**Public agents**: Public agent nodes run DC/OS apps and services through a publicly accessible network.</span></span> 

<span data-ttu-id="bea15-113">Další informace o zabezpečení sítě DC/OS, najdete v článku [DC/OS dokumentaci](https://dcos.io/docs/1.7/administration/securing-your-cluster/).</span><span class="sxs-lookup"><span data-stu-id="bea15-113">For more information about DC/OS network security, see the [DC/OS documentation](https://dcos.io/docs/1.7/administration/securing-your-cluster/).</span></span>

## <a name="deploy-agent-pools"></a><span data-ttu-id="bea15-114">Nasazení agenta fondy</span><span class="sxs-lookup"><span data-stu-id="bea15-114">Deploy agent pools</span></span>

<span data-ttu-id="bea15-115">Fondy agenta DC/OS v Azure Container Service vytvoří následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="bea15-115">The DC/OS agent pools In Azure Container Service are created as follows:</span></span>

* <span data-ttu-id="bea15-116">**Privátní fondu** obsahuje počet uzlů agenta, můžete určit, kdy jste [nasadit cluster DC/OS](container-service-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="bea15-116">The **private pool** contains the number of agent nodes that you specify when you [deploy the DC/OS cluster](container-service-deployment.md).</span></span> 

* <span data-ttu-id="bea15-117">**Veřejné fondu** původně obsahuje předem určený počet uzlů agenta.</span><span class="sxs-lookup"><span data-stu-id="bea15-117">The **public pool** initially contains a predetermined number of agent nodes.</span></span> <span data-ttu-id="bea15-118">Tento fond je automaticky přidáno při zřízení clusteru DC/OS.</span><span class="sxs-lookup"><span data-stu-id="bea15-118">This pool is added automatically when the DC/OS cluster is provisioned.</span></span>

<span data-ttu-id="bea15-119">Fondu privátní a veřejné fondu jsou sady škálování virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="bea15-119">The private pool and the public pool are Azure virtual machine scale sets.</span></span> <span data-ttu-id="bea15-120">Po nasazení můžete nastavit velikost těchto fondů.</span><span class="sxs-lookup"><span data-stu-id="bea15-120">You can resize these pools after deployment.</span></span>

## <a name="use-agent-pools"></a><span data-ttu-id="bea15-121">Používat fondy agenta</span><span class="sxs-lookup"><span data-stu-id="bea15-121">Use agent pools</span></span>
<span data-ttu-id="bea15-122">Ve výchozím nastavení **Marathon** nasadí jakékoli nové aplikace *privátní* agenta uzly.</span><span class="sxs-lookup"><span data-stu-id="bea15-122">By default, **Marathon** deploys any new application to the *private* agent nodes.</span></span> <span data-ttu-id="bea15-123">Je třeba explicitně nasadit aplikaci *veřejné* uzly při vytváření aplikace.</span><span class="sxs-lookup"><span data-stu-id="bea15-123">You have to explicitly deploy the application to the *public* nodes during the creation of the application.</span></span> <span data-ttu-id="bea15-124">Vyberte **volitelné** kartě a zadejte **ACCEPTED** pro **role prostředků** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="bea15-124">Select the **Optional** tab and enter **slave_public** for the **Accepted Resource Roles** value.</span></span> <span data-ttu-id="bea15-125">Tento proces je popsána [sem](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) a v [DC/OS](https://dcos.io/docs/1.7/administration/installing/custom/create-public-agent/) dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="bea15-125">This process is documented [here](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) and in the [DC/OS](https://dcos.io/docs/1.7/administration/installing/custom/create-public-agent/) documentation.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bea15-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bea15-126">Next steps</span></span>
* <span data-ttu-id="bea15-127">Další informace o [Správa kontejnerů Váš DC/OS](container-service-mesos-marathon-ui.md).</span><span class="sxs-lookup"><span data-stu-id="bea15-127">Read more about [managing your DC/OS containers](container-service-mesos-marathon-ui.md).</span></span>

* <span data-ttu-id="bea15-128">Zjistěte, jak [otevření brány firewall](container-service-enable-public-access.md) poskytovaný platformou Azure umožňující veřejný přístup k kontejnery Váš DC/OS.</span><span class="sxs-lookup"><span data-stu-id="bea15-128">Learn how to [open the firewall](container-service-enable-public-access.md) provided by Azure to allow public access to your DC/OS containers.</span></span>

