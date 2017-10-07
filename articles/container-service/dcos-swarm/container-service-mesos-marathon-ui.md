---
title: "aaaManage Azure DC/OS clusteru pomocí uživatelského rozhraní Marathon | Microsoft Docs"
description: "Nasazení služby clusteru Azure Container Service tooan kontejnery pomocí webového uživatelského rozhraní Marathon hello."
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
ms.openlocfilehash: a90180e1b4763e6d2ddfa699ed4b7269f209f728
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-an-azure-container-service-dcos-cluster-through-hello-marathon-web-ui"></a><span data-ttu-id="88497-104">Spravovat clusteru Azure Container Service DC/OS prostřednictvím webového uživatelského rozhraní Marathon hello</span><span class="sxs-lookup"><span data-stu-id="88497-104">Manage an Azure Container Service DC/OS cluster through hello Marathon web UI</span></span>
<span data-ttu-id="88497-105">DC/OS poskytuje prostředí pro nasazování a škálování clusterových úloh a zároveň poskytuje abstrakci používaného hardwaru hello.</span><span class="sxs-lookup"><span data-stu-id="88497-105">DC/OS provides an environment for deploying and scaling clustered workloads, while abstracting hello underlying hardware.</span></span> <span data-ttu-id="88497-106">Nad DC/OS je rozhraní, které spravuje plánování a provádění výpočetních úloh.</span><span class="sxs-lookup"><span data-stu-id="88497-106">On top of DC/OS, there is a framework that manages scheduling and executing compute workloads.</span></span>

<span data-ttu-id="88497-107">Jsou k dispozici pro mnoho populárních úloh rozhraní, tento dokument popisuje, jak tooget začít s nasazením kontejnery pomocí Marathonu.</span><span class="sxs-lookup"><span data-stu-id="88497-107">While frameworks are available for many popular workloads, this document describes how tooget started deploying containers with Marathon.</span></span> 


## <a name="prerequisites"></a><span data-ttu-id="88497-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="88497-108">Prerequisites</span></span>
<span data-ttu-id="88497-109">Než si projdete tyto příklady, budete potřebovat cluster DC/OS nakonfigurovaný v Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="88497-109">Before working through these examples, you need a DC/OS cluster that is configured in Azure Container Service.</span></span> <span data-ttu-id="88497-110">Budete také potřebovat clusteru toothis toohave vzdáleného připojení.</span><span class="sxs-lookup"><span data-stu-id="88497-110">You also need toohave remote connectivity toothis cluster.</span></span> <span data-ttu-id="88497-111">Další informace o těchto položek najdete v tématu hello následující články:</span><span class="sxs-lookup"><span data-stu-id="88497-111">For more information on these items, see hello following articles:</span></span>

* [<span data-ttu-id="88497-112">Nasazení clusteru Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="88497-112">Deploy an Azure Container Service cluster</span></span>](container-service-deployment.md)
* [<span data-ttu-id="88497-113">Připojení clusteru Azure Container Service tooan</span><span class="sxs-lookup"><span data-stu-id="88497-113">Connect tooan Azure Container Service cluster</span></span>](../container-service-connect.md)

> [!NOTE]
> <span data-ttu-id="88497-114">Tento článek předpokládá, že máte k dispozici tunel clusteru DC/OS toohello prostřednictvím místního portu 80.</span><span class="sxs-lookup"><span data-stu-id="88497-114">This article assumes you are tunneling toohello DC/OS cluster through your local port 80.</span></span>
>

## <a name="explore-hello-dcos-ui"></a><span data-ttu-id="88497-115">Prozkoumejte hello uživatelského rozhraní DC/OS</span><span class="sxs-lookup"><span data-stu-id="88497-115">Explore hello DC/OS UI</span></span>
<span data-ttu-id="88497-116">S tunel Secure Shell (SSH) [navázat](../container-service-connect.md), procházet toohttp://localhost/.</span><span class="sxs-lookup"><span data-stu-id="88497-116">With a Secure Shell (SSH) tunnel [established](../container-service-connect.md), browse toohttp://localhost/.</span></span> <span data-ttu-id="88497-117">To načte hello DC/OS webového uživatelského rozhraní a informace o hello clusteru, například využité prostředky, aktivní agenti a spuštěné služby.</span><span class="sxs-lookup"><span data-stu-id="88497-117">This loads hello DC/OS web UI and shows information about hello cluster, such as used resources, active agents, and running services.</span></span>

![Uživatelské rozhraní DC/OS](./media/container-service-mesos-marathon-ui/dcos2.png)

## <a name="explore-hello-marathon-ui"></a><span data-ttu-id="88497-119">Prozkoumejte hello uživatelského rozhraní Marathon</span><span class="sxs-lookup"><span data-stu-id="88497-119">Explore hello Marathon UI</span></span>
<span data-ttu-id="88497-120">toosee hello uživatelského rozhraní Marathon, přejděte toohttp://localhost/marathon.</span><span class="sxs-lookup"><span data-stu-id="88497-120">toosee hello Marathon UI, browse toohttp://localhost/marathon.</span></span> <span data-ttu-id="88497-121">Prostřednictvím této obrazovky můžete spustit nový kontejner nebo jinou aplikaci v clusteru Azure Container Service DC/OS hello.</span><span class="sxs-lookup"><span data-stu-id="88497-121">From this screen, you can start a new container or another application on hello Azure Container Service DC/OS cluster.</span></span> <span data-ttu-id="88497-122">Taky uvidíte informace o spuštěných kontejnerech a aplikacích.</span><span class="sxs-lookup"><span data-stu-id="88497-122">You can also see information about running containers and applications.</span></span>  

![Uživatelské rozhraní Marathon](./media/container-service-mesos-marathon-ui/dcos3.png)

## <a name="deploy-a-docker-formatted-container"></a><span data-ttu-id="88497-124">Nasazení kontejneru formátovaného Dockerem</span><span class="sxs-lookup"><span data-stu-id="88497-124">Deploy a Docker-formatted container</span></span>
<span data-ttu-id="88497-125">Klikněte na tlačítko toodeploy nový kontejner pomocí Marathonu, **vytvořit aplikaci**a zadejte následující informace do formuláře karet hello hello:</span><span class="sxs-lookup"><span data-stu-id="88497-125">toodeploy a new container by using Marathon, click **Create Application**, and enter hello following information into hello form tabs:</span></span>

| <span data-ttu-id="88497-126">Pole</span><span class="sxs-lookup"><span data-stu-id="88497-126">Field</span></span> | <span data-ttu-id="88497-127">Hodnota</span><span class="sxs-lookup"><span data-stu-id="88497-127">Value</span></span> |
| --- | --- |
| <span data-ttu-id="88497-128">ID</span><span class="sxs-lookup"><span data-stu-id="88497-128">ID</span></span> |<span data-ttu-id="88497-129">nginx</span><span class="sxs-lookup"><span data-stu-id="88497-129">nginx</span></span> |
| <span data-ttu-id="88497-130">Memory (Paměť)</span><span class="sxs-lookup"><span data-stu-id="88497-130">Memory</span></span> | <span data-ttu-id="88497-131">32</span><span class="sxs-lookup"><span data-stu-id="88497-131">32</span></span> |
| <span data-ttu-id="88497-132">Image</span><span class="sxs-lookup"><span data-stu-id="88497-132">Image</span></span> |<span data-ttu-id="88497-133">nginx</span><span class="sxs-lookup"><span data-stu-id="88497-133">nginx</span></span> |
| <span data-ttu-id="88497-134">Network (Síť)</span><span class="sxs-lookup"><span data-stu-id="88497-134">Network</span></span> |<span data-ttu-id="88497-135">Bridged (Zapojeno do mostu)</span><span class="sxs-lookup"><span data-stu-id="88497-135">Bridged</span></span> |
| <span data-ttu-id="88497-136">Host Port (Port hostitele)</span><span class="sxs-lookup"><span data-stu-id="88497-136">Host Port</span></span> |<span data-ttu-id="88497-137">80</span><span class="sxs-lookup"><span data-stu-id="88497-137">80</span></span> |
| <span data-ttu-id="88497-138">Protocol (Protokol)</span><span class="sxs-lookup"><span data-stu-id="88497-138">Protocol</span></span> |<span data-ttu-id="88497-139">TCP</span><span class="sxs-lookup"><span data-stu-id="88497-139">TCP</span></span> |

![Nová aplikace uživatelského rozhraní – obecné](./media/container-service-mesos-marathon-ui/dcos4.png)

![Nová aplikace uživatelského rozhraní – kontejner Docker](./media/container-service-mesos-marathon-ui/dcos5.png)

![Nová aplikace uživatelského rozhraní – porty a zjišťování služby](./media/container-service-mesos-marathon-ui/dcos6.png)

<span data-ttu-id="88497-143">Pokud chcete, aby toostatically mapy hello port kontejneru tooa port hello agenta, je třeba toouse režim JSON.</span><span class="sxs-lookup"><span data-stu-id="88497-143">If you want toostatically map hello container port tooa port on hello agent, you need toouse JSON Mode.</span></span> <span data-ttu-id="88497-144">toodo příliš tedy přepínač Průvodce novou aplikací hello**režim JSON** pomocí přepnutí hello.</span><span class="sxs-lookup"><span data-stu-id="88497-144">toodo so, switch hello New Application wizard too**JSON Mode** by using hello toggle.</span></span> <span data-ttu-id="88497-145">Potom zadejte následující nastavení v části hello hello `portMappings` části definice aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="88497-145">Then enter hello following setting under hello `portMappings` section of hello application definition.</span></span> <span data-ttu-id="88497-146">Tento příklad vytvoří vazbu na port 80 hello kontejneru tooport 80 agenta DC/OS hello.</span><span class="sxs-lookup"><span data-stu-id="88497-146">This example binds port 80 of hello container tooport 80 of hello DC/OS agent.</span></span> <span data-ttu-id="88497-147">Po provedení této změny můžete režim JSON v průvodci opět vypnout.</span><span class="sxs-lookup"><span data-stu-id="88497-147">You can switch this wizard out of JSON Mode after you make this change.</span></span>

```none
"hostPort": 80,
```

![Nová aplikace uživatelského rozhraní – příklad u portu číslo 80](./media/container-service-mesos-marathon-ui/dcos13.png)

<span data-ttu-id="88497-149">Pokud chcete, aby tooenable kontroly stavu, nastavte cestu na hello **kontroluje stav** kartě.</span><span class="sxs-lookup"><span data-stu-id="88497-149">If you want tooenable health checks, set a path on hello **Health Checks** tab.</span></span>

![Nová aplikace uživatelského rozhraní – kontroly stavu](./media/container-service-mesos-marathon-ui/dcos_healthcheck.png)

<span data-ttu-id="88497-151">Hello clusteru DC/OS se nasazuje se sadou privátních a veřejných agentů.</span><span class="sxs-lookup"><span data-stu-id="88497-151">hello DC/OS cluster is deployed with set of private and public agents.</span></span> <span data-ttu-id="88497-152">Hello clusteru toobe možné tooaccess aplikací z hello Internet musíte toodeploy hello aplikace tooa veřejného agenta.</span><span class="sxs-lookup"><span data-stu-id="88497-152">For hello cluster toobe able tooaccess applications from hello Internet, you need toodeploy hello applications tooa public agent.</span></span> <span data-ttu-id="88497-153">toodo tedy vyberte hello **volitelné** karta hello Průvodce novou aplikaci a zadejte **ACCEPTED** pro hello **role prostředků**.</span><span class="sxs-lookup"><span data-stu-id="88497-153">toodo so, select hello **Optional** tab of hello New Application wizard and enter **slave_public** for hello **Accepted Resource Roles**.</span></span>

<span data-ttu-id="88497-154">Pak klikněte na **Create Application** (Vytvořit aplikaci).</span><span class="sxs-lookup"><span data-stu-id="88497-154">Then click **Create Application**.</span></span>

![Nová aplikace uživatelského rozhraní – nastavení veřejného agenta](./media/container-service-mesos-marathon-ui/dcos14.png)

<span data-ttu-id="88497-156">Zpět na hlavní stránku Marathonu hello uvidíte stav nasazení hello hello kontejneru.</span><span class="sxs-lookup"><span data-stu-id="88497-156">Back on hello Marathon main page, you can see hello deployment status for hello container.</span></span> <span data-ttu-id="88497-157">Zpočátku uvidíte stav **Deploying** (Nasazování).</span><span class="sxs-lookup"><span data-stu-id="88497-157">Initially you see a status of **Deploying**.</span></span> <span data-ttu-id="88497-158">Po úspěšné nasazení se hello změny stavu příliš**systémem**.</span><span class="sxs-lookup"><span data-stu-id="88497-158">After a successful deployment, hello status changes too**Running**.</span></span>

![Hlavní stránka uživatelského rozhraní Marathon – stav nasazení kontejneru](./media/container-service-mesos-marathon-ui/dcos7.png)

<span data-ttu-id="88497-160">Pokud přepnete zpět toohello DC/OS webového uživatelského rozhraní (http://localhost/), uvidíte, že úloha (v tomto případě kontejner formátovaný) běží na clusteru DC/OS hello.</span><span class="sxs-lookup"><span data-stu-id="88497-160">When you switch back toohello DC/OS web UI (http://localhost/), you see that a task (in this case, a Docker-formatted container) is running on hello DC/OS cluster.</span></span>

![DC/OS webové uživatelské rozhraní – úloha spuštěná na clusteru hello](./media/container-service-mesos-marathon-ui/dcos8.png)

<span data-ttu-id="88497-162">toosee hello uzlu, který hello úloha běží na, klikněte na tlačítko hello **uzly** kartě.</span><span class="sxs-lookup"><span data-stu-id="88497-162">toosee hello cluster node that hello task is running on, click hello **Nodes** tab.</span></span>

![Webové uživatelské rozhraní DC/OS – uzel clusteru úlohy](./media/container-service-mesos-marathon-ui/dcos9.png)

## <a name="reach-hello-container"></a><span data-ttu-id="88497-164">Dosažení hello kontejneru</span><span class="sxs-lookup"><span data-stu-id="88497-164">Reach hello container</span></span>

<span data-ttu-id="88497-165">V tomto příkladu hello aplikace běží na uzlu veřejného agenta.</span><span class="sxs-lookup"><span data-stu-id="88497-165">In this example, hello application is running on a public agent node.</span></span> <span data-ttu-id="88497-166">Nedostanete hello aplikace hello internet procházením toohello agenta plně kvalifikovaný název domény clusteru hello: `http://[DNSPREFIX]agents.[REGION].cloudapp.azure.com`, kde:</span><span class="sxs-lookup"><span data-stu-id="88497-166">You reach hello application from hello internet by browsing toohello agent FQDN of hello cluster: `http://[DNSPREFIX]agents.[REGION].cloudapp.azure.com`, where:</span></span>

* <span data-ttu-id="88497-167">**DNSPREFIX** hello předpona DNS, který jste zadali při nasazení clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="88497-167">**DNSPREFIX** is hello DNS prefix that you provided when you deployed hello cluster.</span></span>
* <span data-ttu-id="88497-168">**OBLAST** hello oblast, ve kterém se nachází vaší skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="88497-168">**REGION** is hello region in which your resource group is located.</span></span>

    ![Nginx z Internetu](./media/container-service-mesos-marathon-ui/nginx.png)


## <a name="next-steps"></a><span data-ttu-id="88497-170">Další kroky</span><span class="sxs-lookup"><span data-stu-id="88497-170">Next steps</span></span>
* [<span data-ttu-id="88497-171">Práce s DC/OS a hello Marathon API</span><span class="sxs-lookup"><span data-stu-id="88497-171">Work with DC/OS and hello Marathon API</span></span>](container-service-mesos-marathon-rest.md)

* <span data-ttu-id="88497-172">Podrobné informace o hello Azure Container Service s Mesos</span><span class="sxs-lookup"><span data-stu-id="88497-172">Deep dive on hello Azure Container Service with Mesos</span></span>

    > [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON203/player]
    > 
