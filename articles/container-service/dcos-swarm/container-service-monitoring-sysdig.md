---
title: aaaMonitor Azure Container Service cluster s Sysdig | Microsoft Docs
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
ms.openlocfilehash: 72f2d3d6f6885f9876fa158b88aae58b84a4610f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-an-azure-container-service-cluster-with-sysdig"></a><span data-ttu-id="badcd-104">Monitorování clusteru služby Azure Container Service pomocí služby Sysdig</span><span class="sxs-lookup"><span data-stu-id="badcd-104">Monitor an Azure Container Service cluster with Sysdig</span></span>
<span data-ttu-id="badcd-105">V tomto článku jsme nasadí Sysdig agenti tooall hello agenta uzlů v clusteru Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="badcd-105">In this article, we will deploy Sysdig agents tooall hello agent nodes in your Azure Container Service cluster.</span></span> <span data-ttu-id="badcd-106">Pro tuto konfiguraci potřebujete účet se službou Sysdig.</span><span class="sxs-lookup"><span data-stu-id="badcd-106">You need an account with Sysdig for this configuration.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="badcd-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="badcd-107">Prerequisites</span></span>
<span data-ttu-id="badcd-108">[Nasaďte](container-service-deployment.md) a [připojte](../container-service-connect.md) cluster nakonfigurovaný službou Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="badcd-108">[Deploy](container-service-deployment.md) and [connect](../container-service-connect.md) a cluster configured by Azure Container Service.</span></span> <span data-ttu-id="badcd-109">Prozkoumejte hello [uživatelského rozhraní Marathon](container-service-mesos-marathon-ui.md).</span><span class="sxs-lookup"><span data-stu-id="badcd-109">Explore hello [Marathon UI](container-service-mesos-marathon-ui.md).</span></span> <span data-ttu-id="badcd-110">Přejděte příliš[http://app.sysdigcloud.com](http://app.sysdigcloud.com) tooset si účet Sysdig cloudu.</span><span class="sxs-lookup"><span data-stu-id="badcd-110">Go too[http://app.sysdigcloud.com](http://app.sysdigcloud.com) tooset up a Sysdig cloud account.</span></span> 

## <a name="sysdig"></a><span data-ttu-id="badcd-111">Sysdig</span><span class="sxs-lookup"><span data-stu-id="badcd-111">Sysdig</span></span>
<span data-ttu-id="badcd-112">Sysdig je monitorování služba, která vám umožní toomonitor kontejnerů v rámci clusteru.</span><span class="sxs-lookup"><span data-stu-id="badcd-112">Sysdig is a monitoring service that allows you toomonitor your containers within your cluster.</span></span> <span data-ttu-id="badcd-113">Sysdig se označuje toohelp při řešení problémů, ale také obsahuje vaše základní monitorování metriky pro procesor, sítě, paměť a vstupně-výstupních operací.</span><span class="sxs-lookup"><span data-stu-id="badcd-113">Sysdig is known toohelp with troubleshooting but it also has your basic monitoring metrics for CPU, Networking, Memory, and I/O.</span></span> <span data-ttu-id="badcd-114">Sysdig umožňuje snadno toosee kontejnery, které pracují hello používá hardest nebo v podstatě hello většina paměti a procesoru.</span><span class="sxs-lookup"><span data-stu-id="badcd-114">Sysdig makes it easy toosee which containers are working hello hardest or essentially using hello most memory and CPU.</span></span> <span data-ttu-id="badcd-115">Toto zobrazení je v části "Přehled", který je aktuálně ve verzi beta hello.</span><span class="sxs-lookup"><span data-stu-id="badcd-115">This view is in hello “Overview” section, which is currently in beta.</span></span> 

![Uživatelské rozhraní služby Sysdig](./media/container-service-monitoring-sysdig/sysdig6.png) 

## <a name="configure-a-sysdig-deployment-with-marathon"></a><span data-ttu-id="badcd-117">Konfigurace nasazení služby Sysdig s uživatelským rozhraním Marathon</span><span class="sxs-lookup"><span data-stu-id="badcd-117">Configure a Sysdig deployment with Marathon</span></span>
<span data-ttu-id="badcd-118">Tyto kroky se dozvíte, jak tooconfigure a Sysdig aplikace tooyour cluster pomocí Marathonu nasadit.</span><span class="sxs-lookup"><span data-stu-id="badcd-118">These steps will show you how tooconfigure and deploy Sysdig applications tooyour cluster with Marathon.</span></span> 

<span data-ttu-id="badcd-119">Přístup přes uživatelské rozhraní DC/OS [http://localhost:80 /](http://localhost:80/) jednou v hello uživatelského rozhraní DC/OS přejděte toohello "Universe", který je na hello dolů, doleva a poté vyhledejte "Sysdig."</span><span class="sxs-lookup"><span data-stu-id="badcd-119">Access your DC/OS UI via [http://localhost:80/](http://localhost:80/) Once in hello DC/OS UI navigate toohello "Universe", which is on hello bottom left and then search for "Sysdig."</span></span>

![Sysdig v rozhraní DC/OS Universe](./media/container-service-monitoring-sysdig/sysdig1.png)

<span data-ttu-id="badcd-121">Nyní toocomplete hello konfigurace nezbytné Sysdig cloudu účet nebo Bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="badcd-121">Now toocomplete hello configuration you need a Sysdig cloud account or a free trial account.</span></span> <span data-ttu-id="badcd-122">Jakmile jste přihlášeni toohello Sysdig cloudu web, klikněte na své uživatelské jméno a na stránce hello byste měli vidět vaší "přístupový klíč."</span><span class="sxs-lookup"><span data-stu-id="badcd-122">Once you're logged in toohello Sysdig cloud website, click on your user name, and on hello page you should see your "Access Key."</span></span> 

![Klíč rozhraní API služby Sysdig](./media/container-service-monitoring-sysdig/sysdig2.png) 

<span data-ttu-id="badcd-124">Potom zadejte přístupový klíč do hello Sysdig konfigurace v rámci hello Universe DC/OS.</span><span class="sxs-lookup"><span data-stu-id="badcd-124">Next enter your Access Key into hello Sysdig configuration within hello DC/OS Universe.</span></span> 

![Konfigurace Sysdig v hello Universe DC/OS](./media/container-service-monitoring-sysdig/sysdig3.png)

<span data-ttu-id="badcd-126">Teď nastavte hello instancí too10000000 tak vždy, když je přidán nový uzel clusteru toohello Sysdig automaticky nasadit agenta toothat nový uzel.</span><span class="sxs-lookup"><span data-stu-id="badcd-126">Now set hello instances too10000000 so whenever a new node is added toohello cluster Sysdig will automatically deploy an agent toothat new node.</span></span> <span data-ttu-id="badcd-127">Toto je toomake dočasné řešení, která je opravdu že sysdig nasadí tooall nové agenty v rámci clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="badcd-127">This is an interim solution toomake sure Sysdig will deploy tooall new agents within hello cluster.</span></span> 

![Konfigurace Sysdig v hello DC/OS Universe-instance](./media/container-service-monitoring-sysdig/sysdig4.png)

<span data-ttu-id="badcd-129">Jakmile jste nainstalovali balíček hello přejděte zpět toohello Sysdig uživatelského rozhraní a budete moct tooexplore hello různých využití metriky pro hello kontejnery v rámci clusteru.</span><span class="sxs-lookup"><span data-stu-id="badcd-129">Once you've installed hello package navigate back toohello Sysdig UI and you'll be able tooexplore hello different usage metrics for hello containers within your cluster.</span></span> 

<span data-ttu-id="badcd-130">Můžete také nainstalovat řídicí panely specifické pro Mesos a Marathon prostřednictvím [průvodce novým řídicím panelem](https://app.sysdigcloud.com/#/dashboards/new).</span><span class="sxs-lookup"><span data-stu-id="badcd-130">You can also install Mesos and Marathon specific dashboards via the [new dashboard wizard](https://app.sysdigcloud.com/#/dashboards/new).</span></span>
