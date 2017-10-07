---
title: fondy agenta aaaDC/OS pro Azure Container Service | Microsoft Docs
description: "Jak fungují hello veřejné a privátní agenta fondy s clusteru služby Azure Container Service DC/OS"
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
ms.openlocfilehash: c7d3889db07cb9908e8b68b668bd8a14ef3c2552
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="dcos-agent-pools-for-azure-container-service"></a><span data-ttu-id="cf7e8-104">Fondy agenta DC/OS pro Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="cf7e8-104">DC/OS agent pools for Azure Container Service</span></span>
<span data-ttu-id="cf7e8-105">Clustery DC/OS v Azure Container Service obsahovat agenta uzly ve dvou fondů, veřejné fondu a privátní fondu.</span><span class="sxs-lookup"><span data-stu-id="cf7e8-105">DC/OS clusters in Azure Container Service contain agent nodes in two pools, a public pool and a private pool.</span></span> <span data-ttu-id="cf7e8-106">Aplikace lze nasadit tooeither fondu, které mají vliv na usnadnění přístupu mezi počítači v kontejneru služby.</span><span class="sxs-lookup"><span data-stu-id="cf7e8-106">An application can be deployed tooeither pool, affecting accessibility between machines in your container service.</span></span> <span data-ttu-id="cf7e8-107">Hello počítače můžou být zveřejněné toohello Internetu (veřejných) nebo interní (soukromý) zachovány.</span><span class="sxs-lookup"><span data-stu-id="cf7e8-107">hello machines can be exposed toohello internet (public) or kept internal (private).</span></span> <span data-ttu-id="cf7e8-108">Tento článek poskytuje stručný přehled proto nejsou veřejné a privátní fondy.</span><span class="sxs-lookup"><span data-stu-id="cf7e8-108">This article gives a brief overview of why there are public and private pools.</span></span>


* <span data-ttu-id="cf7e8-109">**Privátní agenti**: uzly privátní agenta spustit přes síť s směrovat.</span><span class="sxs-lookup"><span data-stu-id="cf7e8-109">**Private agents**: Private agent nodes run through a non-routable network.</span></span> <span data-ttu-id="cf7e8-110">Tato síť je přístupné pouze ze zóny správce hello nebo prostřednictvím hello veřejné zóny hraniční směrovač.</span><span class="sxs-lookup"><span data-stu-id="cf7e8-110">This network is only accessible from hello admin zone or through hello public zone edge router.</span></span> <span data-ttu-id="cf7e8-111">Ve výchozím nastavení spustí DC/OS aplikace na uzlech privátní agenta.</span><span class="sxs-lookup"><span data-stu-id="cf7e8-111">By default, DC/OS launches apps on private agent nodes.</span></span> 

* <span data-ttu-id="cf7e8-112">**Veřejné agenty**: uzly veřejného agenta spustit DC/OS aplikace a služby přes síť s veřejně přístupná.</span><span class="sxs-lookup"><span data-stu-id="cf7e8-112">**Public agents**: Public agent nodes run DC/OS apps and services through a publicly accessible network.</span></span> 

<span data-ttu-id="cf7e8-113">Další informace o zabezpečení sítě DC/OS, najdete v části hello [DC/OS dokumentaci](https://dcos.io/docs/1.7/administration/securing-your-cluster/).</span><span class="sxs-lookup"><span data-stu-id="cf7e8-113">For more information about DC/OS network security, see hello [DC/OS documentation](https://dcos.io/docs/1.7/administration/securing-your-cluster/).</span></span>

## <a name="deploy-agent-pools"></a><span data-ttu-id="cf7e8-114">Nasazení agenta fondy</span><span class="sxs-lookup"><span data-stu-id="cf7e8-114">Deploy agent pools</span></span>

<span data-ttu-id="cf7e8-115">fondy agenta DC/OS Hello v Azure Container Service vytvoří následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="cf7e8-115">hello DC/OS agent pools In Azure Container Service are created as follows:</span></span>

* <span data-ttu-id="cf7e8-116">Hello **privátní fondu** obsahuje hello počet uzlů agenta, můžete určit, kdy jste [nasadit cluster DC/OS hello](container-service-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="cf7e8-116">hello **private pool** contains hello number of agent nodes that you specify when you [deploy hello DC/OS cluster](container-service-deployment.md).</span></span> 

* <span data-ttu-id="cf7e8-117">Hello **veřejné fondu** původně obsahuje předem určený počet uzlů agenta.</span><span class="sxs-lookup"><span data-stu-id="cf7e8-117">hello **public pool** initially contains a predetermined number of agent nodes.</span></span> <span data-ttu-id="cf7e8-118">Tento fond je automaticky přidáno při zřízení clusteru DC/OS hello.</span><span class="sxs-lookup"><span data-stu-id="cf7e8-118">This pool is added automatically when hello DC/OS cluster is provisioned.</span></span>

<span data-ttu-id="cf7e8-119">fond Hello privátní a veřejné fondu hello jsou sady škálování virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="cf7e8-119">hello private pool and hello public pool are Azure virtual machine scale sets.</span></span> <span data-ttu-id="cf7e8-120">Po nasazení můžete nastavit velikost těchto fondů.</span><span class="sxs-lookup"><span data-stu-id="cf7e8-120">You can resize these pools after deployment.</span></span>

## <a name="use-agent-pools"></a><span data-ttu-id="cf7e8-121">Používat fondy agenta</span><span class="sxs-lookup"><span data-stu-id="cf7e8-121">Use agent pools</span></span>
<span data-ttu-id="cf7e8-122">Ve výchozím nastavení **Marathon** nasadí žádné nové aplikace toohello *privátní* agenta uzly.</span><span class="sxs-lookup"><span data-stu-id="cf7e8-122">By default, **Marathon** deploys any new application toohello *private* agent nodes.</span></span> <span data-ttu-id="cf7e8-123">Máte tooexplicitly nasazení toohello aplikace hello *veřejné* uzly během vytváření hello aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="cf7e8-123">You have tooexplicitly deploy hello application toohello *public* nodes during hello creation of hello application.</span></span> <span data-ttu-id="cf7e8-124">Vyberte hello **volitelné** kartě a zadejte **ACCEPTED** pro hello **role prostředků** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="cf7e8-124">Select hello **Optional** tab and enter **slave_public** for hello **Accepted Resource Roles** value.</span></span> <span data-ttu-id="cf7e8-125">Tento proces je popsána [sem](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) a v hello [DC/OS](https://dcos.io/docs/1.7/administration/installing/custom/create-public-agent/) dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="cf7e8-125">This process is documented [here](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) and in hello [DC/OS](https://dcos.io/docs/1.7/administration/installing/custom/create-public-agent/) documentation.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cf7e8-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cf7e8-126">Next steps</span></span>
* <span data-ttu-id="cf7e8-127">Další informace o [Správa kontejnerů Váš DC/OS](container-service-mesos-marathon-ui.md).</span><span class="sxs-lookup"><span data-stu-id="cf7e8-127">Read more about [managing your DC/OS containers](container-service-mesos-marathon-ui.md).</span></span>

* <span data-ttu-id="cf7e8-128">Zjistěte, jak příliš[otevřít bránu firewall hello](container-service-enable-public-access.md) poskytované Azure tooallow veřejný přístup tooyour DC/OS kontejnery.</span><span class="sxs-lookup"><span data-stu-id="cf7e8-128">Learn how too[open hello firewall](container-service-enable-public-access.md) provided by Azure tooallow public access tooyour DC/OS containers.</span></span>

