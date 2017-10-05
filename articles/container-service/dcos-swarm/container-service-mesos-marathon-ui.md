---
title: "Správa clusteru Azure DC/OS pomocí uživatelského rozhraní Marathon | Microsoft Docs"
description: "Využijte webového uživatelského rozhraní Marathon k nasazení kontejnerů do clusteru Azure Container Service."
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
ms.date: 04/04/2017
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: b00088bb005519dc5d533433308c0e3e33c7f433
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="manage-an-azure-container-service-dcos-cluster-through-the-marathon-web-ui"></a><span data-ttu-id="3a35d-104">Správa clusteru Azure Container Service DC/OS přes webové uživatelské rozhraní Marathon</span><span class="sxs-lookup"><span data-stu-id="3a35d-104">Manage an Azure Container Service DC/OS cluster through the Marathon web UI</span></span>
<span data-ttu-id="3a35d-105">DC/OS poskytuje prostředí pro nasazování a škálování clusterových úloh a zároveň poskytuje abstrakci používaného hardwaru.</span><span class="sxs-lookup"><span data-stu-id="3a35d-105">DC/OS provides an environment for deploying and scaling clustered workloads, while abstracting the underlying hardware.</span></span> <span data-ttu-id="3a35d-106">Nad DC/OS je rozhraní, které spravuje plánování a provádění výpočetních úloh.</span><span class="sxs-lookup"><span data-stu-id="3a35d-106">On top of DC/OS, there is a framework that manages scheduling and executing compute workloads.</span></span>

<span data-ttu-id="3a35d-107">Jsou k dispozici pro mnoho populárních úloh rozhraní, tento dokument popisuje, jak začít nasazení kontejnerů pomocí Marathonu.</span><span class="sxs-lookup"><span data-stu-id="3a35d-107">While frameworks are available for many popular workloads, this document describes how to get started deploying containers with Marathon.</span></span> 


## <a name="prerequisites"></a><span data-ttu-id="3a35d-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3a35d-108">Prerequisites</span></span>
<span data-ttu-id="3a35d-109">Než si projdete tyto příklady, budete potřebovat cluster DC/OS nakonfigurovaný v Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="3a35d-109">Before working through these examples, you need a DC/OS cluster that is configured in Azure Container Service.</span></span> <span data-ttu-id="3a35d-110">Kromě toho je nutné mít možnost se k tomuto clusteru připojit vzdáleně.</span><span class="sxs-lookup"><span data-stu-id="3a35d-110">You also need to have remote connectivity to this cluster.</span></span> <span data-ttu-id="3a35d-111">Další informace k těmto záležitostem najdete v těchto článcích:</span><span class="sxs-lookup"><span data-stu-id="3a35d-111">For more information on these items, see the following articles:</span></span>

* [<span data-ttu-id="3a35d-112">Nasazení clusteru Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="3a35d-112">Deploy an Azure Container Service cluster</span></span>](container-service-deployment.md)
* [<span data-ttu-id="3a35d-113">Připojení ke clusteru Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="3a35d-113">Connect to an Azure Container Service cluster</span></span>](../container-service-connect.md)

> [!NOTE]
> <span data-ttu-id="3a35d-114">Tento článek předpokládá, že máte k dispozici tunel na clusteru DC/OS prostřednictvím místního portu 80.</span><span class="sxs-lookup"><span data-stu-id="3a35d-114">This article assumes you are tunneling to the DC/OS cluster through your local port 80.</span></span>
>

## <a name="explore-the-dcos-ui"></a><span data-ttu-id="3a35d-115">Zkoumáme uživatelské rozhraní DC/OS</span><span class="sxs-lookup"><span data-stu-id="3a35d-115">Explore the DC/OS UI</span></span>
<span data-ttu-id="3a35d-116">Po [vytvoření](../container-service-connect.md) tunelu Secure Shell (SSH) přejděte na http://localhost/.</span><span class="sxs-lookup"><span data-stu-id="3a35d-116">With a Secure Shell (SSH) tunnel [established](../container-service-connect.md), browse to http://localhost/.</span></span> <span data-ttu-id="3a35d-117">Načte se webové uživatelské rozhraní DC/OS a zobrazí se informace o clusteru, například využité prostředky, aktivní agenti a spuštěné služby.</span><span class="sxs-lookup"><span data-stu-id="3a35d-117">This loads the DC/OS web UI and shows information about the cluster, such as used resources, active agents, and running services.</span></span>

![Uživatelské rozhraní DC/OS](./media/container-service-mesos-marathon-ui/dcos2.png)

## <a name="explore-the-marathon-ui"></a><span data-ttu-id="3a35d-119">Zkoumáme uživatelské rozhraní Marathon</span><span class="sxs-lookup"><span data-stu-id="3a35d-119">Explore the Marathon UI</span></span>
<span data-ttu-id="3a35d-120">Pokud chcete zobrazit uživatelské rozhraní Marathon, přejděte na http://localhost/marathon.</span><span class="sxs-lookup"><span data-stu-id="3a35d-120">To see the Marathon UI, browse to http://localhost/marathon.</span></span> <span data-ttu-id="3a35d-121">Prostřednictvím této obrazovky můžete spustit nový kontejner nebo jinou aplikaci z clusteru Azure Container Service na bázi DC/OS.</span><span class="sxs-lookup"><span data-stu-id="3a35d-121">From this screen, you can start a new container or another application on the Azure Container Service DC/OS cluster.</span></span> <span data-ttu-id="3a35d-122">Taky uvidíte informace o spuštěných kontejnerech a aplikacích.</span><span class="sxs-lookup"><span data-stu-id="3a35d-122">You can also see information about running containers and applications.</span></span>  

![Uživatelské rozhraní Marathon](./media/container-service-mesos-marathon-ui/dcos3.png)

## <a name="deploy-a-docker-formatted-container"></a><span data-ttu-id="3a35d-124">Nasazení kontejneru formátovaného Dockerem</span><span class="sxs-lookup"><span data-stu-id="3a35d-124">Deploy a Docker-formatted container</span></span>
<span data-ttu-id="3a35d-125">Pokud chcete pomocí Marathonu nasadit nový kontejner, klikněte na **Create Application** (Vytvořit aplikaci) a zadejte na kartách formuláře následující informace:</span><span class="sxs-lookup"><span data-stu-id="3a35d-125">To deploy a new container by using Marathon, click **Create Application**, and enter the following information into the form tabs:</span></span>

| <span data-ttu-id="3a35d-126">Pole</span><span class="sxs-lookup"><span data-stu-id="3a35d-126">Field</span></span> | <span data-ttu-id="3a35d-127">Hodnota</span><span class="sxs-lookup"><span data-stu-id="3a35d-127">Value</span></span> |
| --- | --- |
| <span data-ttu-id="3a35d-128">ID</span><span class="sxs-lookup"><span data-stu-id="3a35d-128">ID</span></span> |<span data-ttu-id="3a35d-129">nginx</span><span class="sxs-lookup"><span data-stu-id="3a35d-129">nginx</span></span> |
| <span data-ttu-id="3a35d-130">Memory (Paměť)</span><span class="sxs-lookup"><span data-stu-id="3a35d-130">Memory</span></span> | <span data-ttu-id="3a35d-131">32</span><span class="sxs-lookup"><span data-stu-id="3a35d-131">32</span></span> |
| <span data-ttu-id="3a35d-132">Image</span><span class="sxs-lookup"><span data-stu-id="3a35d-132">Image</span></span> |<span data-ttu-id="3a35d-133">nginx</span><span class="sxs-lookup"><span data-stu-id="3a35d-133">nginx</span></span> |
| <span data-ttu-id="3a35d-134">Network (Síť)</span><span class="sxs-lookup"><span data-stu-id="3a35d-134">Network</span></span> |<span data-ttu-id="3a35d-135">Bridged (Zapojeno do mostu)</span><span class="sxs-lookup"><span data-stu-id="3a35d-135">Bridged</span></span> |
| <span data-ttu-id="3a35d-136">Host Port (Port hostitele)</span><span class="sxs-lookup"><span data-stu-id="3a35d-136">Host Port</span></span> |<span data-ttu-id="3a35d-137">80</span><span class="sxs-lookup"><span data-stu-id="3a35d-137">80</span></span> |
| <span data-ttu-id="3a35d-138">Protocol (Protokol)</span><span class="sxs-lookup"><span data-stu-id="3a35d-138">Protocol</span></span> |<span data-ttu-id="3a35d-139">TCP</span><span class="sxs-lookup"><span data-stu-id="3a35d-139">TCP</span></span> |

![Nová aplikace uživatelského rozhraní – obecné](./media/container-service-mesos-marathon-ui/dcos4.png)

![Nová aplikace uživatelského rozhraní – kontejner Docker](./media/container-service-mesos-marathon-ui/dcos5.png)

![Nová aplikace uživatelského rozhraní – porty a zjišťování služby](./media/container-service-mesos-marathon-ui/dcos6.png)

<span data-ttu-id="3a35d-143">Pokud chcete staticky namapovat port kontejneru na port agenta, budete muset použít režim JSON.</span><span class="sxs-lookup"><span data-stu-id="3a35d-143">If you want to statically map the container port to a port on the agent, you need to use JSON Mode.</span></span> <span data-ttu-id="3a35d-144">Provedete to tak, že pomocí přepínače přepnete průvodce novou aplikací na **JSON Mode** (Režim JSON).</span><span class="sxs-lookup"><span data-stu-id="3a35d-144">To do so, switch the New Application wizard to **JSON Mode** by using the toggle.</span></span> <span data-ttu-id="3a35d-145">Potom v části `portMappings` definice aplikace zadejte následující nastavení.</span><span class="sxs-lookup"><span data-stu-id="3a35d-145">Then enter the following setting under the `portMappings` section of the application definition.</span></span> <span data-ttu-id="3a35d-146">Tento příklad namapuje port číslo 80 kontejneru na port číslo 80 agenta DC/OS.</span><span class="sxs-lookup"><span data-stu-id="3a35d-146">This example binds port 80 of the container to port 80 of the DC/OS agent.</span></span> <span data-ttu-id="3a35d-147">Po provedení této změny můžete režim JSON v průvodci opět vypnout.</span><span class="sxs-lookup"><span data-stu-id="3a35d-147">You can switch this wizard out of JSON Mode after you make this change.</span></span>

```none
"hostPort": 80,
```

![Nová aplikace uživatelského rozhraní – příklad u portu číslo 80](./media/container-service-mesos-marathon-ui/dcos13.png)

<span data-ttu-id="3a35d-149">Pokud chcete povolit kontroly stavu, nastavte cestu na kartě **Health Checks** (Kontroly stavu).</span><span class="sxs-lookup"><span data-stu-id="3a35d-149">If you want to enable health checks, set a path on the **Health Checks** tab.</span></span>

![Nová aplikace uživatelského rozhraní – kontroly stavu](./media/container-service-mesos-marathon-ui/dcos_healthcheck.png)

<span data-ttu-id="3a35d-151">Cluster DC/OS se nasazuje se sadou privátních a veřejných agentů.</span><span class="sxs-lookup"><span data-stu-id="3a35d-151">The DC/OS cluster is deployed with set of private and public agents.</span></span> <span data-ttu-id="3a35d-152">Pokud chcete, aby měl cluster přístup k aplikacím z internetu, musíte aplikace nasadit s použitím veřejného agenta.</span><span class="sxs-lookup"><span data-stu-id="3a35d-152">For the cluster to be able to access applications from the Internet, you need to deploy the applications to a public agent.</span></span> <span data-ttu-id="3a35d-153">Uděláte to následovně: V průvodci novou aplikací vyberete kartu **Optional** (Volitelné) a do **Accepted Resource Roles** (Povolené role prostředků) zadáte **slave_public**.</span><span class="sxs-lookup"><span data-stu-id="3a35d-153">To do so, select the **Optional** tab of the New Application wizard and enter **slave_public** for the **Accepted Resource Roles**.</span></span>

<span data-ttu-id="3a35d-154">Pak klikněte na **Create Application** (Vytvořit aplikaci).</span><span class="sxs-lookup"><span data-stu-id="3a35d-154">Then click **Create Application**.</span></span>

![Nová aplikace uživatelského rozhraní – nastavení veřejného agenta](./media/container-service-mesos-marathon-ui/dcos14.png)

<span data-ttu-id="3a35d-156">Když se vrátíte na hlavní stránku Marathonu, uvidíte stav nasazení daného kontejneru.</span><span class="sxs-lookup"><span data-stu-id="3a35d-156">Back on the Marathon main page, you can see the deployment status for the container.</span></span> <span data-ttu-id="3a35d-157">Zpočátku uvidíte stav **Deploying** (Nasazování).</span><span class="sxs-lookup"><span data-stu-id="3a35d-157">Initially you see a status of **Deploying**.</span></span> <span data-ttu-id="3a35d-158">Po úspěšném nasazení se stav změní na **Running** (spuštěno).</span><span class="sxs-lookup"><span data-stu-id="3a35d-158">After a successful deployment, the status changes to **Running**.</span></span>

![Hlavní stránka uživatelského rozhraní Marathon – stav nasazení kontejneru](./media/container-service-mesos-marathon-ui/dcos7.png)

<span data-ttu-id="3a35d-160">Když přepnete zpět do webového uživatelského rozhraní DC/OS (http://localhost/), uvidíte, že úloha (v tomto případě kontejner formátovaný Dockerem) je spuštěná na clusteru DC/OS.</span><span class="sxs-lookup"><span data-stu-id="3a35d-160">When you switch back to the DC/OS web UI (http://localhost/), you see that a task (in this case, a Docker-formatted container) is running on the DC/OS cluster.</span></span>

![Webové uživatelské rozhraní DC/OS – úloha spuštěná na clusteru](./media/container-service-mesos-marathon-ui/dcos8.png)

<span data-ttu-id="3a35d-162">Pokud chcete zobrazit uzel clusteru, na kterém je úloha spuštěná, klikněte na kartu **Nodes** (Uzly).</span><span class="sxs-lookup"><span data-stu-id="3a35d-162">To see the cluster node that the task is running on, click the **Nodes** tab.</span></span>

![Webové uživatelské rozhraní DC/OS – uzel clusteru úlohy](./media/container-service-mesos-marathon-ui/dcos9.png)

## <a name="reach-the-container"></a><span data-ttu-id="3a35d-164">Dosažení kontejneru</span><span class="sxs-lookup"><span data-stu-id="3a35d-164">Reach the container</span></span>

<span data-ttu-id="3a35d-165">V tomto příkladu aplikace běží na uzlu veřejného agenta.</span><span class="sxs-lookup"><span data-stu-id="3a35d-165">In this example, the application is running on a public agent node.</span></span> <span data-ttu-id="3a35d-166">Nedostanete aplikaci z Internetu procházením agenta plně kvalifikovaný název domény clusteru: `http://[DNSPREFIX]agents.[REGION].cloudapp.azure.com`, kde:</span><span class="sxs-lookup"><span data-stu-id="3a35d-166">You reach the application from the internet by browsing to the agent FQDN of the cluster: `http://[DNSPREFIX]agents.[REGION].cloudapp.azure.com`, where:</span></span>

* <span data-ttu-id="3a35d-167">**DNSPREFIX** je předpona DNS zadaná ve chvíli, kdy jste nasadili cluster.</span><span class="sxs-lookup"><span data-stu-id="3a35d-167">**DNSPREFIX** is the DNS prefix that you provided when you deployed the cluster.</span></span>
* <span data-ttu-id="3a35d-168">**REGION** je oblast, ve které je umístěna skupina prostředků.</span><span class="sxs-lookup"><span data-stu-id="3a35d-168">**REGION** is the region in which your resource group is located.</span></span>

    ![Nginx z Internetu](./media/container-service-mesos-marathon-ui/nginx.png)


## <a name="next-steps"></a><span data-ttu-id="3a35d-170">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3a35d-170">Next steps</span></span>
* [<span data-ttu-id="3a35d-171">Práce s DC/OS a rozhraním API Marathonu</span><span class="sxs-lookup"><span data-stu-id="3a35d-171">Work with DC/OS and the Marathon API</span></span>](container-service-mesos-marathon-rest.md)

* <span data-ttu-id="3a35d-172">Podrobná prohlídka služby Azure Container Service s Mesos</span><span class="sxs-lookup"><span data-stu-id="3a35d-172">Deep dive on the Azure Container Service with Mesos</span></span>

    > [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON203/player]
    > 
