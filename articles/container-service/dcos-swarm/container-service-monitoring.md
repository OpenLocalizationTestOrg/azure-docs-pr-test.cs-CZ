---
title: cluster Azure DC/OS aaaMonitor - Datadog | Microsoft Docs
description: "Monitorování clusteru Azure Container Service s Datadog. Použijte hello DC/OS webového uživatelského rozhraní toodeploy hello Datadog agenti tooyour clusteru."
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
ms.openlocfilehash: 10268c04b5c2ef393429e706ed4a467fff80f718
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-an-azure-container-service-dcos-cluster-with-datadog"></a><span data-ttu-id="f13ae-105">Monitorování clusteru Azure Container Service DC/OS s Datadog</span><span class="sxs-lookup"><span data-stu-id="f13ae-105">Monitor an Azure Container Service DC/OS cluster with Datadog</span></span>
<span data-ttu-id="f13ae-106">V tomto článku jsme nasadí Datadog agenti tooall hello agenta uzlů v clusteru Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="f13ae-106">In this article we will deploy Datadog agents tooall hello agent nodes in your Azure Container Service cluster.</span></span> <span data-ttu-id="f13ae-107">Pro tuto konfiguraci budete potřebovat účet s Datadog.</span><span class="sxs-lookup"><span data-stu-id="f13ae-107">You will need an account with Datadog for this configuration.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="f13ae-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f13ae-108">Prerequisites</span></span>
<span data-ttu-id="f13ae-109">[Nasaďte](container-service-deployment.md) a [připojte](../container-service-connect.md) cluster nakonfigurovaný službou Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="f13ae-109">[Deploy](container-service-deployment.md) and [connect](../container-service-connect.md) a cluster configured by Azure Container Service.</span></span> <span data-ttu-id="f13ae-110">Prozkoumejte hello [uživatelského rozhraní Marathon](container-service-mesos-marathon-ui.md).</span><span class="sxs-lookup"><span data-stu-id="f13ae-110">Explore hello [Marathon UI](container-service-mesos-marathon-ui.md).</span></span> <span data-ttu-id="f13ae-111">Přejděte příliš[http://datadoghq.com](http://datadoghq.com) tooset Datadog účet.</span><span class="sxs-lookup"><span data-stu-id="f13ae-111">Go too[http://datadoghq.com](http://datadoghq.com) tooset up a Datadog account.</span></span> 

## <a name="datadog"></a><span data-ttu-id="f13ae-112">Datadog</span><span class="sxs-lookup"><span data-stu-id="f13ae-112">Datadog</span></span>
<span data-ttu-id="f13ae-113">Datadog je monitorování služba, která shromažďuje data monitorování z kontejnerů v rámci clusteru Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="f13ae-113">Datadog is a monitoring service that gathers monitoring data from your containers within your Azure Container Service cluster.</span></span> <span data-ttu-id="f13ae-114">Datadog má řídicí panel integrace Dockeru kde můžete zobrazit konkrétní metriky v rámci kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="f13ae-114">Datadog has a Docker Integration Dashboard where you can see specific metrics within your containers.</span></span> <span data-ttu-id="f13ae-115">Metriky shromážděná z kontejnerů jsou uspořádané podle využití procesoru, paměti, síťové a vstupně-výstupní operace.</span><span class="sxs-lookup"><span data-stu-id="f13ae-115">Metrics gathered from your containers are organized by CPU, Memory, Network and I/O.</span></span> <span data-ttu-id="f13ae-116">Datadog rozdělí metriky na kontejnery a obrázků.</span><span class="sxs-lookup"><span data-stu-id="f13ae-116">Datadog splits metrics into containers and images.</span></span> <span data-ttu-id="f13ae-117">Příkladem jaké hello uživatelského rozhraní vypadá takto: pro procesor využití je menší než.</span><span class="sxs-lookup"><span data-stu-id="f13ae-117">An example of what hello UI looks like for CPU usage is below.</span></span>

![Datadog uživatelského rozhraní](./media/container-service-monitoring/datadog4.png)

## <a name="configure-a-datadog-deployment-with-marathon"></a><span data-ttu-id="f13ae-119">Konfigurace nasazení Datadog pomocí Marathonu</span><span class="sxs-lookup"><span data-stu-id="f13ae-119">Configure a Datadog deployment with Marathon</span></span>
<span data-ttu-id="f13ae-120">Tyto kroky se dozvíte, jak tooconfigure a Datadog aplikace tooyour cluster pomocí Marathonu nasadit.</span><span class="sxs-lookup"><span data-stu-id="f13ae-120">These steps will show you how tooconfigure and deploy Datadog applications tooyour cluster with Marathon.</span></span> 

<span data-ttu-id="f13ae-121">Přístup přes uživatelské rozhraní DC/OS [http://localhost:80 /](http://localhost:80/).</span><span class="sxs-lookup"><span data-stu-id="f13ae-121">Access your DC/OS UI via [http://localhost:80/](http://localhost:80/).</span></span> <span data-ttu-id="f13ae-122">Jednou v hello uživatelského rozhraní DC/OS přejděte toohello "Universe", který je na hello dolů, doleva a poté vyhledejte "Datadog" a klikněte na možnost "Nainstalovat."</span><span class="sxs-lookup"><span data-stu-id="f13ae-122">Once in hello DC/OS UI navigate toohello "Universe" which is on hello bottom left and then search for "Datadog" and click "Install."</span></span>

![Balíček Datadog v rámci hello Universe DC/OS](./media/container-service-monitoring/datadog1.png)

<span data-ttu-id="f13ae-124">Nyní toocomplete hello konfigurace budete potřebovat účet Datadog nebo Bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="f13ae-124">Now toocomplete hello configuration you will need a Datadog account or a free trial account.</span></span> <span data-ttu-id="f13ae-125">Jakmile jste se přihlásili toohello Datadog webu vypadají toohello doleva a přejděte tooIntegrations -> pak [rozhraní API](https://app.datadoghq.com/account/settings#api).</span><span class="sxs-lookup"><span data-stu-id="f13ae-125">Once you're logged in toohello Datadog website look toohello left and go tooIntegrations -> then [APIs](https://app.datadoghq.com/account/settings#api).</span></span> 

![Klíč rozhraní API Datadog](./media/container-service-monitoring/datadog2.png)

<span data-ttu-id="f13ae-127">Potom zadejte klíč rozhraní API do hello Datadog konfigurace v rámci hello Universe DC/OS.</span><span class="sxs-lookup"><span data-stu-id="f13ae-127">Next enter your API key into hello Datadog configuration within hello DC/OS Universe.</span></span> 

![Konfigurace Datadog v hello Universe DC/OS](./media/container-service-monitoring/datadog3.png) 

<span data-ttu-id="f13ae-129">V hello výše konfigurace instance jsou nastaveny too10000000, tak pokaždé, když je přidán nový uzel clusteru toohello Datadog automaticky nasadí do agenta toothat uzlu.</span><span class="sxs-lookup"><span data-stu-id="f13ae-129">In hello above configuration instances are set too10000000 so whenever a new node is added toohello cluster Datadog will automatically deploy an agent toothat node.</span></span> <span data-ttu-id="f13ae-130">Toto je předběžná řešení.</span><span class="sxs-lookup"><span data-stu-id="f13ae-130">This is an interim solution.</span></span> <span data-ttu-id="f13ae-131">Po instalaci balíčku hello můžete přejít zpět toohello Datadog webu a najít "[řídicí panely](https://app.datadoghq.com/dash/list)."</span><span class="sxs-lookup"><span data-stu-id="f13ae-131">Once you've installed hello package you should navigate back toohello Datadog website and find "[Dashboards](https://app.datadoghq.com/dash/list)."</span></span> <span data-ttu-id="f13ae-132">Zde se zobrazí vlastní a integrace řídicí panely.</span><span class="sxs-lookup"><span data-stu-id="f13ae-132">From there you will see Custom and Integration Dashboards.</span></span> <span data-ttu-id="f13ae-133">Hello [řídicí panel Docker](https://app.datadoghq.com/screen/integration/docker) budou mít všechny metriky hello kontejner pro monitorování vašeho clusteru potřebujete.</span><span class="sxs-lookup"><span data-stu-id="f13ae-133">hello [Docker dashboard](https://app.datadoghq.com/screen/integration/docker) will have all hello container metrics you need for monitoring your cluster.</span></span> 

