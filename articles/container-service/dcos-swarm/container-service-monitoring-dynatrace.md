---
title: "Cluster Azure DC/OS monitorování - Dynatrace | Microsoft Docs"
description: "Monitorování clusteru Azure Container Service DC/OS s Dynatrace. Nasaďte Dynatrace OneAgent pomocí řídicího panelu DC/OS."
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
ms.openlocfilehash: 6fa23728680779e33eda7bb9aa8a01b9cad9a82b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-an-azure-container-service-dcos-cluster-with-dynatrace-saasmanaged"></a><span data-ttu-id="0802c-105">Monitorování clusteru Azure Container Service DC/OS s Dynatrace SaaS/spravované</span><span class="sxs-lookup"><span data-stu-id="0802c-105">Monitor an Azure Container Service DC/OS cluster with Dynatrace SaaS/Managed</span></span>
<span data-ttu-id="0802c-106">V tomto článku jsme ukazují, jak nasadit [Dynatrace](https://www.dynatrace.com/) OneAgent monitorovat všechny agenta uzly v clusteru Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="0802c-106">In this article, we show you how to deploy the [Dynatrace](https://www.dynatrace.com/) OneAgent to monitor all the agent nodes in your Azure Container Service cluster.</span></span> <span data-ttu-id="0802c-107">Potřebujete účet s Dynatrace SaaS/spravované pro tuto konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="0802c-107">You need an account with Dynatrace SaaS/Managed for this configuration.</span></span> 

## <a name="dynatrace-saasmanaged"></a><span data-ttu-id="0802c-108">Dynatrace SaaS/spravované</span><span class="sxs-lookup"><span data-stu-id="0802c-108">Dynatrace SaaS/Managed</span></span>
<span data-ttu-id="0802c-109">Dynatrace je řešení monitorování cloudu nativní pro vysoce dynamické kontejneru a prostředí clusteru.</span><span class="sxs-lookup"><span data-stu-id="0802c-109">Dynatrace is a cloud-native monitoring solution for highly dynamic container and cluster environments.</span></span> <span data-ttu-id="0802c-110">Umožňuje lépe optimalizovat nasazení kontejnerů a přidělení paměti s použitím data o využití v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="0802c-110">It allows you to better optimize your container deployments and memory allocations by using real-time usage data.</span></span> <span data-ttu-id="0802c-111">Je schopný automaticky přesným rozpoznáním aplikace a infrastrukturu problémů tím, že poskytuje automatizované monitorování standardních hodnot, korelace problém a zjištění hlavní příčiny.</span><span class="sxs-lookup"><span data-stu-id="0802c-111">It is capable of automatically pinpointing application and infrastructure issues by providing automated baselining, problem correlation, and root-cause detection.</span></span>

<span data-ttu-id="0802c-112">Následující obrázek znázorňuje Dynatrace uživatelského rozhraní:</span><span class="sxs-lookup"><span data-stu-id="0802c-112">The following figure shows the Dynatrace UI:</span></span>

![Dynatrace uživatelského rozhraní](./media/container-service-monitoring-dynatrace/dynatrace.png)

## <a name="prerequisites"></a><span data-ttu-id="0802c-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="0802c-114">Prerequisites</span></span> 
<span data-ttu-id="0802c-115">[Nasazení](container-service-deployment.md) a [připojit](./../container-service-connect.md) do clusteru s podporou konfigurovat tak, že Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="0802c-115">[Deploy](container-service-deployment.md) and [connect](./../container-service-connect.md) to a cluster configured by Azure Container Service.</span></span> <span data-ttu-id="0802c-116">Prozkoumejte [uživatelské rozhraní Marathon](container-service-mesos-marathon-ui.md).</span><span class="sxs-lookup"><span data-stu-id="0802c-116">Explore the [Marathon UI](container-service-mesos-marathon-ui.md).</span></span> <span data-ttu-id="0802c-117">Přejděte na [https://www.dynatrace.com/trial/](https://www.dynatrace.com/trial/) nastavit účet Dynatrace SaaS.</span><span class="sxs-lookup"><span data-stu-id="0802c-117">Go to [https://www.dynatrace.com/trial/](https://www.dynatrace.com/trial/) to set up a Dynatrace SaaS account.</span></span>  

## <a name="configure-a-dynatrace-deployment-with-marathon"></a><span data-ttu-id="0802c-118">Konfigurace nasazení Dynatrace pomocí Marathonu</span><span class="sxs-lookup"><span data-stu-id="0802c-118">Configure a Dynatrace deployment with Marathon</span></span>
<span data-ttu-id="0802c-119">Tyto kroky ukazují, jak nakonfigurovat a nasadit aplikace Dynatrace ke clusteru pomocí Marathonu.</span><span class="sxs-lookup"><span data-stu-id="0802c-119">These steps show you how to configure and deploy Dynatrace applications to your cluster with Marathon.</span></span>

1. <span data-ttu-id="0802c-120">Přístup přes uživatelské rozhraní DC/OS [http://localhost:80 /](http://localhost:80/).</span><span class="sxs-lookup"><span data-stu-id="0802c-120">Access your DC/OS UI via [http://localhost:80/](http://localhost:80/).</span></span> <span data-ttu-id="0802c-121">Jednou v uživatelském rozhraní DC/OS, přejděte na **Universe** kartě a poté vyhledejte **Dynatrace**.</span><span class="sxs-lookup"><span data-stu-id="0802c-121">Once in the DC/OS UI, navigate to the **Universe** tab and then search for **Dynatrace**.</span></span>

    ![Dynatrace v Universe DC/OS](./media/container-service-monitoring-dynatrace/dynatrace-universe.png)

2. <span data-ttu-id="0802c-123">K dokončení konfigurace, potřebujete účet Dynatrace SaaS nebo Bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="0802c-123">To complete the configuration, you need a Dynatrace SaaS account or a free trial account.</span></span> <span data-ttu-id="0802c-124">Po přihlášení do řídicího panelu Dynatrace vybrat **nasazení Dynatrace**.</span><span class="sxs-lookup"><span data-stu-id="0802c-124">Once you log into the Dynatrace dashboard, select **Deploy Dynatrace**.</span></span>

    ![Nastavení Dynatrace PaaS integrace](./media/container-service-monitoring-dynatrace/setup-paas.png)

3. <span data-ttu-id="0802c-126">Na stránce vyberte **nastavení integrace PaaS**.</span><span class="sxs-lookup"><span data-stu-id="0802c-126">On the page, select **Set up PaaS integration**.</span></span> 

    ![Rozhraní API Dynatrace tokenu](./media/container-service-monitoring-dynatrace/api-token.png) 

4. <span data-ttu-id="0802c-128">Zadejte token vaše rozhraní API do konfigurace Dynatrace OneAgent v rámci Universe DC/OS.</span><span class="sxs-lookup"><span data-stu-id="0802c-128">Enter your API token into the Dynatrace OneAgent configuration within the DC/OS Universe.</span></span> 

    ![Konfigurace Dynatrace OneAgent v Universe DC/OS](./media/container-service-monitoring-dynatrace/dynatrace-config.png)

5. <span data-ttu-id="0802c-130">Nastavte instance na počet uzlů, který chcete spustit.</span><span class="sxs-lookup"><span data-stu-id="0802c-130">Set the instances to the number of nodes you intend to run.</span></span> <span data-ttu-id="0802c-131">Nastaví větší číslo také funguje, ale DC/OS bude nadále pokoušet najít nové instance toto číslo je ve skutečnosti ukončen.</span><span class="sxs-lookup"><span data-stu-id="0802c-131">Setting a higher number also works, but DC/OS will keep trying to find new instances until that number is actually reached.</span></span> <span data-ttu-id="0802c-132">Pokud dáváte přednost, můžete tuto vlastnost nastavit na hodnotu jako 1000000.</span><span class="sxs-lookup"><span data-stu-id="0802c-132">If you prefer, you can also set this to a value like 1000000.</span></span> <span data-ttu-id="0802c-133">V takovém případě vždy, když do clusteru je přidán nový uzel, Dynatrace automaticky nasadí agent do tohoto nového uzlu za cenu neustále se snažíte nasadit další instance DC/OS.</span><span class="sxs-lookup"><span data-stu-id="0802c-133">In this case, whenever a new node is added to the cluster, Dynatrace automatically deploys an agent to that new node, at the price of DC/OS constantly trying to deploy further instances.</span></span>

    ![Konfigurace Dynatrace v DC/OS Universe-instance](./media/container-service-monitoring-dynatrace/dynatrace-config2.png)

## <a name="next-steps"></a><span data-ttu-id="0802c-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0802c-135">Next steps</span></span>

<span data-ttu-id="0802c-136">Jakmile jste nainstalovali balíček, přejděte zpět na řídicí panel Dynatrace.</span><span class="sxs-lookup"><span data-stu-id="0802c-136">Once you've installed the package, navigate back to the Dynatrace dashboard.</span></span> <span data-ttu-id="0802c-137">Použití jiné metriky pro kontejnery můžete prozkoumat v rámci clusteru.</span><span class="sxs-lookup"><span data-stu-id="0802c-137">You can explore the different usage metrics for the containers within your cluster.</span></span> 