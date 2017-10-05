---
title: "Načíst kontejnery vyrovnávání v clusteru Azure DC/OS | Microsoft Docs"
description: "Vyrovnávání zatížení v rámci několika kontejnerů na clusteru Azure Container Service DC/OS."
services: container-service
documentationcenter: 
author: rgardler
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Kontejnery, mikroslužby, DC/OS, Azure"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/02/2017
ms.author: rogardle
ms.custom: mvc
ms.openlocfilehash: 78725c9d23e13d307821a188028ef573d1def038
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="load-balance-containers-in-an-azure-container-service-dcos-cluster"></a><span data-ttu-id="2c6bc-104">Kontejnery Vyrovnávání zatížení v clusteru služby Azure Container Service DC/OS</span><span class="sxs-lookup"><span data-stu-id="2c6bc-104">Load balance containers in an Azure Container Service DC/OS cluster</span></span>
<span data-ttu-id="2c6bc-105">V tomto článku jsme zjistit, jak vytvořit interní nástroj v DC/OS spravované Azure Container Service pomocí Marathon-LB.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-105">In this article, we explore how to create an internal load balancer in a DC/OS managed Azure Container Service using Marathon-LB.</span></span> <span data-ttu-id="2c6bc-106">Tato konfigurace umožňuje škálování aplikací vodorovně.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-106">This configuration enables you to scale your applications horizontally.</span></span> <span data-ttu-id="2c6bc-107">Také umožňuje využít výhod agenta veřejné a privátní clustery umístěním nástroje pro vyrovnávání zatížení na vaše kontejnery aplikací v clusteru, privátní a veřejné clusteru.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-107">It also allows you to take advantage of the public and private agent clusters by placing your load balancers on the public cluster and your application containers on the private cluster.</span></span> <span data-ttu-id="2c6bc-108">V tomto kurzu jste:</span><span class="sxs-lookup"><span data-stu-id="2c6bc-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2c6bc-109">Konfigurace služby Vyrovnávání zatížení Marathon</span><span class="sxs-lookup"><span data-stu-id="2c6bc-109">Configure a Marathon Load Balancer</span></span>
> * <span data-ttu-id="2c6bc-110">Nasazení aplikace pomocí nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="2c6bc-110">Deploy an application using the load balancer</span></span>
> * <span data-ttu-id="2c6bc-111">Konfigurace a nástroj pro vyrovnávání zatížení Azure</span><span class="sxs-lookup"><span data-stu-id="2c6bc-111">Configure and Azure load balancer</span></span>

<span data-ttu-id="2c6bc-112">Je třeba cluster služby ACS DC/OS, pokud chcete provést kroky v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-112">You need an ACS DC/OS cluster to complete the steps in this tutorial.</span></span> <span data-ttu-id="2c6bc-113">V případě potřeby [tento ukázkový skript](./../kubernetes/scripts/container-service-cli-deploy-dcos.md) můžete vytvořit za vás.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-113">If needed, [this script sample](./../kubernetes/scripts/container-service-cli-deploy-dcos.md) can create one for you.</span></span>

<span data-ttu-id="2c6bc-114">Tento kurz vyžaduje Azure CLI verze 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-114">This tutorial requires the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="2c6bc-115">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-115">Run `az --version` to find the version.</span></span> <span data-ttu-id="2c6bc-116">Pokud potřebujete upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="2c6bc-116">If you need to upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="load-balancing-overview"></a><span data-ttu-id="2c6bc-117">Přehled Vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="2c6bc-117">Load balancing overview</span></span>

<span data-ttu-id="2c6bc-118">Existují dvě vrstvy Vyrovnávání zatížení v clusteru služby Azure Container Service DC/OS:</span><span class="sxs-lookup"><span data-stu-id="2c6bc-118">There are two load-balancing layers in an Azure Container Service DC/OS cluster:</span></span> 

<span data-ttu-id="2c6bc-119">**Azure nástroj pro vyrovnávání zatížení** poskytuje veřejné vstupní body (ty, které koncovým uživatelům přístup k).</span><span class="sxs-lookup"><span data-stu-id="2c6bc-119">**Azure Load Balancer** provides public entry points (the ones that end users access).</span></span> <span data-ttu-id="2c6bc-120">Azure LB automaticky poskytuje Azure Container Service a je ve výchozím nastavení nakonfigurován tak, aby vystavit port 80 a 443 8080.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-120">An Azure LB is provided automatically by Azure Container Service and is, by default, configured to expose port 80, 443 and 8080.</span></span>

<span data-ttu-id="2c6bc-121">**Vyrovnávání zatížení (marathon-lb) Marathon** trasy příchozí požadavky na kontejner instancí, které tyto žádosti o služby.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-121">**The Marathon Load Balancer (marathon-lb)** routes inbound requests to container instances that service these requests.</span></span> <span data-ttu-id="2c6bc-122">Škály kontejnerů, které poskytují naše webové služby, marathon-lb dynamicky přizpůsobení.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-122">As we scale the containers providing our web service, the marathon-lb dynamically adapts.</span></span> <span data-ttu-id="2c6bc-123">Tento nástroj pro vyrovnávání zatížení není dostupné ve výchozím nastavení v kontejneru služby, ale lze snadno nainstalovat.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-123">This load balancer is not provided by default in your Container Service, but it is easy to install.</span></span>

## <a name="configure-marathon-load-balancer"></a><span data-ttu-id="2c6bc-124">Konfigurace pro vyrovnávání zatížení Marathon</span><span class="sxs-lookup"><span data-stu-id="2c6bc-124">Configure Marathon Load Balancer</span></span>

<span data-ttu-id="2c6bc-125">Marathon Load Balancer se sám dynamicky rekonfiguruje na základě kontejnerů, které jste nasadili.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-125">Marathon Load Balancer dynamically reconfigures itself based on the containers that you've deployed.</span></span> <span data-ttu-id="2c6bc-126">Je také odolný vůči ztrátě kontejneru nebo agenta – Pokud k tomu dojde, restartuje kontejner na jiném místě Apache Mesos a marathon-lb přizpůsobení.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-126">It's also resilient to the loss of a container or an agent - if this occurs, Apache Mesos restarts the container elsewhere and marathon-lb adapts.</span></span>

<span data-ttu-id="2c6bc-127">Spusťte následující příkaz pro instalaci nástroje pro vyrovnávání zatížení marathon v clusteru veřejného agenta.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-127">Run the following command to install the marathon load balancer on the public agent's cluster.</span></span>

```azurecli-interactive
dcos package install marathon-lb
```

## <a name="deploy-load-balanced-application"></a><span data-ttu-id="2c6bc-128">Nasazení aplikace skupinu s vyrovnáváním zatížení</span><span class="sxs-lookup"><span data-stu-id="2c6bc-128">Deploy load balanced application</span></span>

<span data-ttu-id="2c6bc-129">Jakmile máme balíček marathon-lb, můžeme nasadit kontejner aplikace, u kterého chceme vyrovnávat zatížení.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-129">Now that we have the marathon-lb package, we can deploy an application container that we wish to load balance.</span></span> 

<span data-ttu-id="2c6bc-130">Nejdřív získáte plně kvalifikovaný název domény veřejně vystavené agentů.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-130">First, get the FQDN of the publicly exposed agents.</span></span>

```azurecli-interactive
az acs list --resource-group myResourceGroup --query "[0].agentPoolProfiles[0].fqdn" --output tsv
```

<span data-ttu-id="2c6bc-131">Dále vytvořte soubor s názvem *hello web.json* a zkopírujte následující obsah.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-131">Next, create a file named *hello-web.json* and copy in the following contents.</span></span> <span data-ttu-id="2c6bc-132">`HAPROXY_0_VHOST` Popisek je třeba aktualizovat pomocí plně kvalifikovaný název domény agentů DC/OS.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-132">The `HAPROXY_0_VHOST` label needs to be updated with the FQDN of the DC/OS agents.</span></span> 

```json
{
  "id": "web",
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "yeasy/simple-web",
      "network": "BRIDGE",
      "portMappings": [
        { "hostPort": 0, "containerPort": 80, "servicePort": 10000 }
      ],
      "forcePullImage":true
    }
  },
  "instances": 3,
  "cpus": 0.1,
  "mem": 65,
  "healthChecks": [{
      "protocol": "HTTP",
      "path": "/",
      "portIndex": 0,
      "timeoutSeconds": 10,
      "gracePeriodSeconds": 10,
      "intervalSeconds": 2,
      "maxConsecutiveFailures": 10
  }],
  "labels":{
    "HAPROXY_GROUP":"external",
    "HAPROXY_0_VHOST":"YOUR FQDN",
    "HAPROXY_0_MODE":"http"
  }
}
```

<span data-ttu-id="2c6bc-133">Pomocí rozhraní příkazového řádku DC/OS a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-133">Use the DC/OS CLI to run the application.</span></span> <span data-ttu-id="2c6bc-134">Ve výchozím nastavení nasadí Marathonu aplikace do privátní clusteru.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-134">By default Marathon deploys the the applicaton to the private cluster.</span></span> <span data-ttu-id="2c6bc-135">To znamená, že výše nasazení je pouze přístupné přes nástroj pro vyrovnávání zatížení, který je obvykle toto chování žádoucí.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-135">This means that the above deployment is only accessible via your load balancer, which is usually the desired behavior.</span></span>

```azurecli-interactive
dcos marathon app add hello-web.json
```

<span data-ttu-id="2c6bc-136">Jakmile je aplikace nasazená, přejděte na plně kvalifikovaný název clusteru agenta zobrazíte skupinu s vyrovnáváním zatížení aplikace.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-136">Once the application has been deployed, browse to the FQDN of the agent cluster to view load balanced application.</span></span>

![Bitové kopie aplikace skupinu s vyrovnáváním zatížení](./media/container-service-load-balancing/lb-app.png)

## <a name="configure-azure-load-balancer"></a><span data-ttu-id="2c6bc-138">Konfigurace pro vyrovnávání zatížení Azure</span><span class="sxs-lookup"><span data-stu-id="2c6bc-138">Configure Azure Load Balancer</span></span>

<span data-ttu-id="2c6bc-139">Služba Azure Load Balancer ve výchozím nastavení zpřístupňuje porty 80, 8080 a 443.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-139">By default, Azure Load Balancer exposes ports 80, 8080, and 443.</span></span> <span data-ttu-id="2c6bc-140">Pokud používáte některý z těchto tří portů (jako my v příkladu výše), není třeba provádět žádnou další akci.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-140">If you're using one of these three ports (as we do in the above example), then there is nothing you need to do.</span></span> <span data-ttu-id="2c6bc-141">Nyní byste měli mít dosáhl agenta nástroj pro vyrovnávání zatížení je plně kvalifikovaný název domény a pokaždé, když aktualizujete, můžete budete použít jeden ze tří webových serverů v kruhového dotazování.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-141">You should be able to hit your agent load balancer's FQDN, and each time you refresh, you'll hit one of your three web servers in a round-robin fashion.</span></span> 

<span data-ttu-id="2c6bc-142">Pokud používáte jiný port, budete muset přidat pravidlo kruhového dotazování a najít na Vyrovnávání zatížení pro port, který jste použili.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-142">If you use a different port, you need to add a round-robin rule and a probe on the load balancer for the port that you used.</span></span> <span data-ttu-id="2c6bc-143">To lze udělat z [rozhraní příkazového řádku Azure](../../azure-resource-manager/xplat-cli-azure-resource-manager.md) pomocí příkazů `azure network lb rule create` a `azure network lb probe create`.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-143">You can do this from the [Azure CLI](../../azure-resource-manager/xplat-cli-azure-resource-manager.md), with the commands `azure network lb rule create` and `azure network lb probe create`.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2c6bc-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2c6bc-144">Next steps</span></span>

<span data-ttu-id="2c6bc-145">V tomto kurzu jste se dozvěděli o vyrovnávání zatížení v rámci služby ACS se Marathon i Azure Vyrovnávání zatížení, včetně následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="2c6bc-145">In this tutorial, you learned about load balancing in ACS with both the Marathon and Azure load balancers including the following actions:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2c6bc-146">Konfigurace služby Vyrovnávání zatížení Marathon</span><span class="sxs-lookup"><span data-stu-id="2c6bc-146">Configure a Marathon Load Balancer</span></span>
> * <span data-ttu-id="2c6bc-147">Nasazení aplikace pomocí nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="2c6bc-147">Deploy an application using the load balancer</span></span>
> * <span data-ttu-id="2c6bc-148">Konfigurace a nástroj pro vyrovnávání zatížení Azure</span><span class="sxs-lookup"><span data-stu-id="2c6bc-148">Configure and Azure load balancer</span></span>

<span data-ttu-id="2c6bc-149">Přechodu na v dalším kurzu se dozvíte o integraci Azure storage s DC/OS v Azure.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-149">Advance to the next tutorial to learn about integrating Azure storage with DC/OS in Azure.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="2c6bc-150">Azure připojit sdílenou složku v clusteru DC/OS</span><span class="sxs-lookup"><span data-stu-id="2c6bc-150">Mount Azure File Share in DC/OS cluster</span></span>](container-service-dcos-fileshare.md)