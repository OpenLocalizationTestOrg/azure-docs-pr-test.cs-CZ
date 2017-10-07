---
title: cluster Azure DC/OS aaaMonitor - Dynatrace | Microsoft Docs
description: "Monitorování clusteru Azure Container Service DC/OS s Dynatrace. Nasaďte hello Dynatrace OneAgent pomocí řídicího panelu hello DC/OS."
services: container-service
documentationcenter: 
author: MartinGoodwell
manager: 
editor: 
tags: acs, azure-container-service
keywords: Kontejnery, DC/OS, Azure
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/13/2016
ms.author: rogardle
ms.custom: mvc
ms.openlocfilehash: 9e2e2d1c8b454422d1db30dac7c385d31d336853
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-an-azure-container-service-dcos-cluster-with-dynatrace-saasmanaged"></a><span data-ttu-id="75881-105">Monitorování clusteru Azure Container Service DC/OS s Dynatrace SaaS/spravované</span><span class="sxs-lookup"><span data-stu-id="75881-105">Monitor an Azure Container Service DC/OS cluster with Dynatrace SaaS/Managed</span></span>
<span data-ttu-id="75881-106">V tomto článku jsme ukazují, jak toodeploy hello [Dynatrace](https://www.dynatrace.com/) OneAgent toomonitor všechny hello agenta uzlů v clusteru Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="75881-106">In this article, we show you how toodeploy hello [Dynatrace](https://www.dynatrace.com/) OneAgent toomonitor all hello agent nodes in your Azure Container Service cluster.</span></span> <span data-ttu-id="75881-107">Potřebujete účet s Dynatrace SaaS/spravované pro tuto konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="75881-107">You need an account with Dynatrace SaaS/Managed for this configuration.</span></span> 

## <a name="dynatrace-saasmanaged"></a><span data-ttu-id="75881-108">Dynatrace SaaS/spravované</span><span class="sxs-lookup"><span data-stu-id="75881-108">Dynatrace SaaS/Managed</span></span>
<span data-ttu-id="75881-109">Dynatrace je řešení monitorování cloudu nativní pro vysoce dynamické kontejneru a prostředí clusteru.</span><span class="sxs-lookup"><span data-stu-id="75881-109">Dynatrace is a cloud-native monitoring solution for highly dynamic container and cluster environments.</span></span> <span data-ttu-id="75881-110">Umožňuje vám toobetter optimalizovat nasazení kontejnerů a přidělení paměti s použitím data o využití v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="75881-110">It allows you toobetter optimize your container deployments and memory allocations by using real-time usage data.</span></span> <span data-ttu-id="75881-111">Je schopný automaticky přesným rozpoznáním aplikace a infrastrukturu problémů tím, že poskytuje automatizované monitorování standardních hodnot, korelace problém a zjištění hlavní příčiny.</span><span class="sxs-lookup"><span data-stu-id="75881-111">It is capable of automatically pinpointing application and infrastructure issues by providing automated baselining, problem correlation, and root-cause detection.</span></span>

<span data-ttu-id="75881-112">Hello následující obrázek ukazuje hello Dynatrace uživatelského rozhraní:</span><span class="sxs-lookup"><span data-stu-id="75881-112">hello following figure shows hello Dynatrace UI:</span></span>

![Dynatrace uživatelského rozhraní](./media/container-service-monitoring-dynatrace/dynatrace.png)

## <a name="prerequisites"></a><span data-ttu-id="75881-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="75881-114">Prerequisites</span></span> 
<span data-ttu-id="75881-115">[Nasazení](container-service-deployment.md) a [připojit](./../container-service-connect.md) tooa cluster nakonfigurovaný v Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="75881-115">[Deploy](container-service-deployment.md) and [connect](./../container-service-connect.md) tooa cluster configured by Azure Container Service.</span></span> <span data-ttu-id="75881-116">Prozkoumejte hello [uživatelského rozhraní Marathon](container-service-mesos-marathon-ui.md).</span><span class="sxs-lookup"><span data-stu-id="75881-116">Explore hello [Marathon UI](container-service-mesos-marathon-ui.md).</span></span> <span data-ttu-id="75881-117">Přejděte příliš[https://www.dynatrace.com/trial/](https://www.dynatrace.com/trial/) tooset Dynatrace SaaS účet.</span><span class="sxs-lookup"><span data-stu-id="75881-117">Go too[https://www.dynatrace.com/trial/](https://www.dynatrace.com/trial/) tooset up a Dynatrace SaaS account.</span></span>  

## <a name="configure-a-dynatrace-deployment-with-marathon"></a><span data-ttu-id="75881-118">Konfigurace nasazení Dynatrace pomocí Marathonu</span><span class="sxs-lookup"><span data-stu-id="75881-118">Configure a Dynatrace deployment with Marathon</span></span>
<span data-ttu-id="75881-119">Tyto kroky vám ukážou, jak tooconfigure a Dynatrace aplikace tooyour cluster pomocí Marathonu nasadit.</span><span class="sxs-lookup"><span data-stu-id="75881-119">These steps show you how tooconfigure and deploy Dynatrace applications tooyour cluster with Marathon.</span></span>

1. <span data-ttu-id="75881-120">Přístup přes uživatelské rozhraní DC/OS [http://localhost:80 /](http://localhost:80/).</span><span class="sxs-lookup"><span data-stu-id="75881-120">Access your DC/OS UI via [http://localhost:80/](http://localhost:80/).</span></span> <span data-ttu-id="75881-121">Jednou v hello uživatelského rozhraní DC/OS, přejděte toohello **Universe** kartě a poté vyhledejte **Dynatrace**.</span><span class="sxs-lookup"><span data-stu-id="75881-121">Once in hello DC/OS UI, navigate toohello **Universe** tab and then search for **Dynatrace**.</span></span>

    ![Dynatrace v Universe DC/OS](./media/container-service-monitoring-dynatrace/dynatrace-universe.png)

2. <span data-ttu-id="75881-123">Konfigurace hello toocomplete, potřebujete účet Dynatrace SaaS nebo Bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="75881-123">toocomplete hello configuration, you need a Dynatrace SaaS account or a free trial account.</span></span> <span data-ttu-id="75881-124">Po přihlášení do řídicího panelu Dynatrace hello vybrat **nasazení Dynatrace**.</span><span class="sxs-lookup"><span data-stu-id="75881-124">Once you log into hello Dynatrace dashboard, select **Deploy Dynatrace**.</span></span>

    ![Nastavení Dynatrace PaaS integrace](./media/container-service-monitoring-dynatrace/setup-paas.png)

3. <span data-ttu-id="75881-126">Na stránce hello vyberte **nastavení integrace PaaS**.</span><span class="sxs-lookup"><span data-stu-id="75881-126">On hello page, select **Set up PaaS integration**.</span></span> 

    ![Rozhraní API Dynatrace tokenu](./media/container-service-monitoring-dynatrace/api-token.png) 

4. <span data-ttu-id="75881-128">Zadejte token vaše rozhraní API do hello Dynatrace OneAgent konfigurace v rámci hello Universe DC/OS.</span><span class="sxs-lookup"><span data-stu-id="75881-128">Enter your API token into hello Dynatrace OneAgent configuration within hello DC/OS Universe.</span></span> 

    ![Konfigurace Dynatrace OneAgent v hello Universe DC/OS](./media/container-service-monitoring-dynatrace/dynatrace-config.png)

5. <span data-ttu-id="75881-130">Nastavit hello instancí toohello počet uzlů, který chcete toorun.</span><span class="sxs-lookup"><span data-stu-id="75881-130">Set hello instances toohello number of nodes you intend toorun.</span></span> <span data-ttu-id="75881-131">Nastaví větší číslo také funguje, ale DC/OS se zkusit nové instance toofind toto číslo je ve skutečnosti ukončen.</span><span class="sxs-lookup"><span data-stu-id="75881-131">Setting a higher number also works, but DC/OS will keep trying toofind new instances until that number is actually reached.</span></span> <span data-ttu-id="75881-132">Pokud dáváte přednost, můžete také nastavit tuto hodnotu tooa jako 1000000.</span><span class="sxs-lookup"><span data-stu-id="75881-132">If you prefer, you can also set this tooa value like 1000000.</span></span> <span data-ttu-id="75881-133">V takovém případě vždy, když toohello clusteru je přidán nový uzel, Dynatrace automaticky nasadí do agenta toothat nového uzlu, za cenu hello neustále pokusu o další instance toodeploy DC/OS.</span><span class="sxs-lookup"><span data-stu-id="75881-133">In this case, whenever a new node is added toohello cluster, Dynatrace automatically deploys an agent toothat new node, at hello price of DC/OS constantly trying toodeploy further instances.</span></span>

    ![Konfigurace Dynatrace v hello DC/OS Universe-instance](./media/container-service-monitoring-dynatrace/dynatrace-config2.png)

## <a name="next-steps"></a><span data-ttu-id="75881-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="75881-135">Next steps</span></span>

<span data-ttu-id="75881-136">Jakmile jste nainstalovali balíček hello, přejděte zpět toohello Dynatrace řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="75881-136">Once you've installed hello package, navigate back toohello Dynatrace dashboard.</span></span> <span data-ttu-id="75881-137">V rámci clusteru můžete prozkoumat hello různých využití metriky pro kontejnery hello.</span><span class="sxs-lookup"><span data-stu-id="75881-137">You can explore hello different usage metrics for hello containers within your cluster.</span></span> 
