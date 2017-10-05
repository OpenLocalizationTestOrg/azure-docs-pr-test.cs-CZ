---
title: "Cluster Azure DC/OS monitorování - Datadog | Microsoft Docs"
description: "Monitorování clusteru Azure Container Service s Datadog. K nasazení agentů Datadog ke clusteru pomocí webového uživatelského rozhraní DC/OS."
services: container-service
documentationcenter: 
author: sauryadas
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: Kontejnery Azure DC/OS, Docker Swarm
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure
ms.date: 07/28/2016
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: 9dd451f994940d7cc3a59bd7fd08a8f067345e34
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-an-azure-container-service-dcos-cluster-with-datadog"></a><span data-ttu-id="e8cdc-105">Monitorování clusteru Azure Container Service DC/OS s Datadog</span><span class="sxs-lookup"><span data-stu-id="e8cdc-105">Monitor an Azure Container Service DC/OS cluster with Datadog</span></span>
<span data-ttu-id="e8cdc-106">V tomto článku jsme nasadí Datadog agentů na všechny uzly agenta v clusteru Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="e8cdc-106">In this article we will deploy Datadog agents to all the agent nodes in your Azure Container Service cluster.</span></span> <span data-ttu-id="e8cdc-107">Pro tuto konfiguraci budete potřebovat účet s Datadog.</span><span class="sxs-lookup"><span data-stu-id="e8cdc-107">You will need an account with Datadog for this configuration.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="e8cdc-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e8cdc-108">Prerequisites</span></span>
<span data-ttu-id="e8cdc-109">[Nasaďte](container-service-deployment.md) a [připojte](../container-service-connect.md) cluster nakonfigurovaný službou Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="e8cdc-109">[Deploy](container-service-deployment.md) and [connect](../container-service-connect.md) a cluster configured by Azure Container Service.</span></span> <span data-ttu-id="e8cdc-110">Prozkoumejte [uživatelské rozhraní Marathon](container-service-mesos-marathon-ui.md).</span><span class="sxs-lookup"><span data-stu-id="e8cdc-110">Explore the [Marathon UI](container-service-mesos-marathon-ui.md).</span></span> <span data-ttu-id="e8cdc-111">Přejděte na [http://datadoghq.com](http://datadoghq.com) nastavit účet Datadog.</span><span class="sxs-lookup"><span data-stu-id="e8cdc-111">Go to [http://datadoghq.com](http://datadoghq.com) to set up a Datadog account.</span></span> 

## <a name="datadog"></a><span data-ttu-id="e8cdc-112">Datadog</span><span class="sxs-lookup"><span data-stu-id="e8cdc-112">Datadog</span></span>
<span data-ttu-id="e8cdc-113">Datadog je monitorování služba, která shromažďuje data monitorování z kontejnerů v rámci clusteru Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="e8cdc-113">Datadog is a monitoring service that gathers monitoring data from your containers within your Azure Container Service cluster.</span></span> <span data-ttu-id="e8cdc-114">Datadog má řídicí panel integrace Dockeru kde můžete zobrazit konkrétní metriky v rámci kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="e8cdc-114">Datadog has a Docker Integration Dashboard where you can see specific metrics within your containers.</span></span> <span data-ttu-id="e8cdc-115">Metriky shromážděná z kontejnerů jsou uspořádané podle využití procesoru, paměti, síťové a vstupně-výstupní operace.</span><span class="sxs-lookup"><span data-stu-id="e8cdc-115">Metrics gathered from your containers are organized by CPU, Memory, Network and I/O.</span></span> <span data-ttu-id="e8cdc-116">Datadog rozdělí metriky na kontejnery a obrázků.</span><span class="sxs-lookup"><span data-stu-id="e8cdc-116">Datadog splits metrics into containers and images.</span></span> <span data-ttu-id="e8cdc-117">Příklad vzhled uživatelského rozhraní pro využití procesoru je pod.</span><span class="sxs-lookup"><span data-stu-id="e8cdc-117">An example of what the UI looks like for CPU usage is below.</span></span>

![Datadog uživatelského rozhraní](./media/container-service-monitoring/datadog4.png)

## <a name="configure-a-datadog-deployment-with-marathon"></a><span data-ttu-id="e8cdc-119">Konfigurace nasazení Datadog pomocí Marathonu</span><span class="sxs-lookup"><span data-stu-id="e8cdc-119">Configure a Datadog deployment with Marathon</span></span>
<span data-ttu-id="e8cdc-120">Tyto kroky vám ukáže, jak nakonfigurovat a nasadit aplikace Datadog ke clusteru pomocí Marathonu.</span><span class="sxs-lookup"><span data-stu-id="e8cdc-120">These steps will show you how to configure and deploy Datadog applications to your cluster with Marathon.</span></span> 

<span data-ttu-id="e8cdc-121">Přístup přes uživatelské rozhraní DC/OS [http://localhost:80 /](http://localhost:80/).</span><span class="sxs-lookup"><span data-stu-id="e8cdc-121">Access your DC/OS UI via [http://localhost:80/](http://localhost:80/).</span></span> <span data-ttu-id="e8cdc-122">Jednou v uživatelském rozhraní DC/OS přejděte na "základní soubor" což je ve spodní levé a poté vyhledejte "Datadog" a klikněte na možnost "Nainstalovat."</span><span class="sxs-lookup"><span data-stu-id="e8cdc-122">Once in the DC/OS UI navigate to the "Universe" which is on the bottom left and then search for "Datadog" and click "Install."</span></span>

![Balíček Datadog v rámci Universe DC/OS](./media/container-service-monitoring/datadog1.png)

<span data-ttu-id="e8cdc-124">Teď k dokončení konfigurace budete potřebovat účet Datadog nebo Bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="e8cdc-124">Now to complete the configuration you will need a Datadog account or a free trial account.</span></span> <span data-ttu-id="e8cdc-125">Po přihlášeni vzhledu webu Datadog doleva a přejděte do integrace -> pak [rozhraní API](https://app.datadoghq.com/account/settings#api).</span><span class="sxs-lookup"><span data-stu-id="e8cdc-125">Once you're logged in to the Datadog website look to the left and go to Integrations -> then [APIs](https://app.datadoghq.com/account/settings#api).</span></span> 

![Klíč rozhraní API Datadog](./media/container-service-monitoring/datadog2.png)

<span data-ttu-id="e8cdc-127">Potom zadejte klíč rozhraní API do Datadog konfigurace v rámci Universe DC/OS.</span><span class="sxs-lookup"><span data-stu-id="e8cdc-127">Next enter your API key into the Datadog configuration within the DC/OS Universe.</span></span> 

![Konfigurace Datadog v Universe DC/OS](./media/container-service-monitoring/datadog3.png) 

<span data-ttu-id="e8cdc-129">Ve výše uvedené konfigurace instance jsou nastaveny na 10000000 tak vždy, když je přidán nový uzel clusteru Datadog automaticky nasadí agent na tomto uzlu.</span><span class="sxs-lookup"><span data-stu-id="e8cdc-129">In the above configuration instances are set to 10000000 so whenever a new node is added to the cluster Datadog will automatically deploy an agent to that node.</span></span> <span data-ttu-id="e8cdc-130">Toto je předběžná řešení.</span><span class="sxs-lookup"><span data-stu-id="e8cdc-130">This is an interim solution.</span></span> <span data-ttu-id="e8cdc-131">Jakmile jste nainstalovali balíček, měli byste přejděte zpět na web Datadog a najít "[řídicí panely](https://app.datadoghq.com/dash/list)."</span><span class="sxs-lookup"><span data-stu-id="e8cdc-131">Once you've installed the package you should navigate back to the Datadog website and find "[Dashboards](https://app.datadoghq.com/dash/list)."</span></span> <span data-ttu-id="e8cdc-132">Zde se zobrazí vlastní a integrace řídicí panely.</span><span class="sxs-lookup"><span data-stu-id="e8cdc-132">From there you will see Custom and Integration Dashboards.</span></span> <span data-ttu-id="e8cdc-133">[Řídicí panel Docker](https://app.datadoghq.com/screen/integration/docker) budou mít všechny metriky kontejner pro monitorování vašeho clusteru potřebujete.</span><span class="sxs-lookup"><span data-stu-id="e8cdc-133">The [Docker dashboard](https://app.datadoghq.com/screen/integration/docker) will have all the container metrics you need for monitoring your cluster.</span></span> 

