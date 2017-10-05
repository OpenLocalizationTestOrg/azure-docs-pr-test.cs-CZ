---
title: "Monitorování clusteru služby Azure Container Service pomocí služby Sysdig | Dokumentace Microsoftu"
description: "Cluster služby Azure Container Service můžete monitorovat pomocí služby Sysdig."
services: container-service
documentationcenter: 
author: sauryadas
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: Kontejnery, DC/OS, Azure
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2016
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: e61001161e632a5d2e513107e30f1eaf06103989
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-an-azure-container-service-cluster-with-sysdig"></a><span data-ttu-id="d1ba9-104">Monitorování clusteru služby Azure Container Service pomocí služby Sysdig</span><span class="sxs-lookup"><span data-stu-id="d1ba9-104">Monitor an Azure Container Service cluster with Sysdig</span></span>
<span data-ttu-id="d1ba9-105">V tomto článku nasadíme na všechny agentské uzly v clusteru služby Azure Container Service agenty služby Sysdig.</span><span class="sxs-lookup"><span data-stu-id="d1ba9-105">In this article, we will deploy Sysdig agents to all the agent nodes in your Azure Container Service cluster.</span></span> <span data-ttu-id="d1ba9-106">Pro tuto konfiguraci potřebujete účet se službou Sysdig.</span><span class="sxs-lookup"><span data-stu-id="d1ba9-106">You need an account with Sysdig for this configuration.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="d1ba9-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d1ba9-107">Prerequisites</span></span>
<span data-ttu-id="d1ba9-108">[Nasaďte](container-service-deployment.md) a [připojte](../container-service-connect.md) cluster nakonfigurovaný službou Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="d1ba9-108">[Deploy](container-service-deployment.md) and [connect](../container-service-connect.md) a cluster configured by Azure Container Service.</span></span> <span data-ttu-id="d1ba9-109">Prozkoumejte [uživatelské rozhraní Marathon](container-service-mesos-marathon-ui.md).</span><span class="sxs-lookup"><span data-stu-id="d1ba9-109">Explore the [Marathon UI](container-service-mesos-marathon-ui.md).</span></span> <span data-ttu-id="d1ba9-110">Přejděte na [http://app.sysdigcloud.com](http://app.sysdigcloud.com) a vytvořte si cloudový účet služby Sysdig.</span><span class="sxs-lookup"><span data-stu-id="d1ba9-110">Go to [http://app.sysdigcloud.com](http://app.sysdigcloud.com) to set up a Sysdig cloud account.</span></span> 

## <a name="sysdig"></a><span data-ttu-id="d1ba9-111">Sysdig</span><span class="sxs-lookup"><span data-stu-id="d1ba9-111">Sysdig</span></span>
<span data-ttu-id="d1ba9-112">Sysdig je monitorovací služba, která vám umožňuje monitorovat kontejnery v rámci vašeho clusteru.</span><span class="sxs-lookup"><span data-stu-id="d1ba9-112">Sysdig is a monitoring service that allows you to monitor your containers within your cluster.</span></span> <span data-ttu-id="d1ba9-113">Služba Sysdig pomáhá s odstraňováním potíží, ale obsahuje taky základní monitorovací metriky procesoru, sítí, paměti a vstupně-výstupních procesů.</span><span class="sxs-lookup"><span data-stu-id="d1ba9-113">Sysdig is known to help with troubleshooting but it also has your basic monitoring metrics for CPU, Networking, Memory, and I/O.</span></span> <span data-ttu-id="d1ba9-114">Služba Sysdig nabízí přehled o tom, které kontejnery jsou nejvytíženější nebo využívají nejvíc paměti a výkonu procesoru.</span><span class="sxs-lookup"><span data-stu-id="d1ba9-114">Sysdig makes it easy to see which containers are working the hardest or essentially using the most memory and CPU.</span></span> <span data-ttu-id="d1ba9-115">To zjistíte v části „Přehled“, který je v současné době dostupný v beta verzi.</span><span class="sxs-lookup"><span data-stu-id="d1ba9-115">This view is in the “Overview” section, which is currently in beta.</span></span> 

![Uživatelské rozhraní služby Sysdig](./media/container-service-monitoring-sysdig/sysdig6.png) 

## <a name="configure-a-sysdig-deployment-with-marathon"></a><span data-ttu-id="d1ba9-117">Konfigurace nasazení služby Sysdig s uživatelským rozhraním Marathon</span><span class="sxs-lookup"><span data-stu-id="d1ba9-117">Configure a Sysdig deployment with Marathon</span></span>
<span data-ttu-id="d1ba9-118">Tento postup vám ukáže, jak nakonfigurovat aplikace služby Sysdig a nasadit je do clusteru pomocí Marathonu.</span><span class="sxs-lookup"><span data-stu-id="d1ba9-118">These steps will show you how to configure and deploy Sysdig applications to your cluster with Marathon.</span></span> 

<span data-ttu-id="d1ba9-119">Otevřete uživatelské rozhraní DC/OS prostřednictvím adresy [http://localhost:80/](http://localhost:80/). V uživatelském rozhraní DC/OS potom vlevo dole přejděte na položku „Universe“ vyhledejte „Sysdig“.</span><span class="sxs-lookup"><span data-stu-id="d1ba9-119">Access your DC/OS UI via [http://localhost:80/](http://localhost:80/) Once in the DC/OS UI navigate to the "Universe", which is on the bottom left and then search for "Sysdig."</span></span>

![Sysdig v rozhraní DC/OS Universe](./media/container-service-monitoring-sysdig/sysdig1.png)

<span data-ttu-id="d1ba9-121">K dokončení konfigurace budete potřebovat cloudový účet služby Sysdig nebo bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="d1ba9-121">Now to complete the configuration you need a Sysdig cloud account or a free trial account.</span></span> <span data-ttu-id="d1ba9-122">Po přihlášení na web cloudu Sysdig klikněte na uživatelské jméno. Zobrazí se stránka, na které byste měli najít svůj „přístupový klíč“.</span><span class="sxs-lookup"><span data-stu-id="d1ba9-122">Once you're logged in to the Sysdig cloud website, click on your user name, and on the page you should see your "Access Key."</span></span> 

![Klíč rozhraní API služby Sysdig](./media/container-service-monitoring-sysdig/sysdig2.png) 

<span data-ttu-id="d1ba9-124">Svůj přístupový klíč zadejte do konfigurace služby Sysdig v rozhraní DC/OS Universe.</span><span class="sxs-lookup"><span data-stu-id="d1ba9-124">Next enter your Access Key into the Sysdig configuration within the DC/OS Universe.</span></span> 

![Konfigurace služby Sysdig v rozhraní DC/OS Universe](./media/container-service-monitoring-sysdig/sysdig3.png)

<span data-ttu-id="d1ba9-126">Teď nastavte instance na hodnotu 10000000, aby služba Sysdig při každém přidání nového uzlu do clusteru automaticky do tohoto nového uzlu nasadila agenta.</span><span class="sxs-lookup"><span data-stu-id="d1ba9-126">Now set the instances to 10000000 so whenever a new node is added to the cluster Sysdig will automatically deploy an agent to that new node.</span></span> <span data-ttu-id="d1ba9-127">Je to dočasné řešení, kterým zajistíte, aby se služba Sysdig nasadila do všech nových agentů v clusteru.</span><span class="sxs-lookup"><span data-stu-id="d1ba9-127">This is an interim solution to make sure Sysdig will deploy to all new agents within the cluster.</span></span> 

![Konfigurace služby Sysdig v rozhraní DC/OS Universe – instance](./media/container-service-monitoring-sysdig/sysdig4.png)

<span data-ttu-id="d1ba9-129">Po instalaci balíčku přejděte zpátky do uživatelského rozhraní služby Sysdig. Teď už budete moc prozkoumávat různé metriky využití kontejnerů v clusteru.</span><span class="sxs-lookup"><span data-stu-id="d1ba9-129">Once you've installed the package navigate back to the Sysdig UI and you'll be able to explore the different usage metrics for the containers within your cluster.</span></span> 

<span data-ttu-id="d1ba9-130">Můžete také nainstalovat řídicí panely specifické pro Mesos a Marathon prostřednictvím [průvodce novým řídicím panelem](https://app.sysdigcloud.com/#/dashboards/new).</span><span class="sxs-lookup"><span data-stu-id="d1ba9-130">You can also install Mesos and Marathon specific dashboards via the [new dashboard wizard](https://app.sysdigcloud.com/#/dashboards/new).</span></span>
