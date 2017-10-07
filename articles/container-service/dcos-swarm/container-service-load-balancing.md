---
title: "kontejnery vyrovnávání aaaLoad v clusteru Azure DC/OS | Microsoft Docs"
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
ms.openlocfilehash: 2249cb06880cdb7e9a3aa94c0750c6a27316d349
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="load-balance-containers-in-an-azure-container-service-dcos-cluster"></a><span data-ttu-id="b3c80-104">Kontejnery Vyrovnávání zatížení v clusteru služby Azure Container Service DC/OS</span><span class="sxs-lookup"><span data-stu-id="b3c80-104">Load balance containers in an Azure Container Service DC/OS cluster</span></span>
<span data-ttu-id="b3c80-105">V tomto článku jsme prozkoumejte spravováni Azure Container Service pomocí Marathon-LB toocreate interní zátěže v DC/OS.</span><span class="sxs-lookup"><span data-stu-id="b3c80-105">In this article, we explore how toocreate an internal load balancer in a DC/OS managed Azure Container Service using Marathon-LB.</span></span> <span data-ttu-id="b3c80-106">Tato konfigurace umožňuje vám tooscale vodorovně vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="b3c80-106">This configuration enables you tooscale your applications horizontally.</span></span> <span data-ttu-id="b3c80-107">Můžete taky využít tootake hello veřejné a privátní agenta clusterů tím, že nástroje pro vyrovnávání zatížení v clusteru veřejné hello a vaše kontejnery aplikací v clusteru privátní hello.</span><span class="sxs-lookup"><span data-stu-id="b3c80-107">It also allows you tootake advantage of hello public and private agent clusters by placing your load balancers on hello public cluster and your application containers on hello private cluster.</span></span> <span data-ttu-id="b3c80-108">V tomto kurzu jste:</span><span class="sxs-lookup"><span data-stu-id="b3c80-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b3c80-109">Konfigurace služby Vyrovnávání zatížení Marathon</span><span class="sxs-lookup"><span data-stu-id="b3c80-109">Configure a Marathon Load Balancer</span></span>
> * <span data-ttu-id="b3c80-110">Nasazení aplikace pomocí nástroje pro vyrovnávání zatížení hello</span><span class="sxs-lookup"><span data-stu-id="b3c80-110">Deploy an application using hello load balancer</span></span>
> * <span data-ttu-id="b3c80-111">Konfigurace a nástroj pro vyrovnávání zatížení Azure</span><span class="sxs-lookup"><span data-stu-id="b3c80-111">Configure and Azure load balancer</span></span>

<span data-ttu-id="b3c80-112">Budete potřebovat DC/OS ACS clusteru toocomplete hello kroky v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="b3c80-112">You need an ACS DC/OS cluster toocomplete hello steps in this tutorial.</span></span> <span data-ttu-id="b3c80-113">V případě potřeby [tento ukázkový skript](./../kubernetes/scripts/container-service-cli-deploy-dcos.md) můžete vytvořit za vás.</span><span class="sxs-lookup"><span data-stu-id="b3c80-113">If needed, [this script sample](./../kubernetes/scripts/container-service-cli-deploy-dcos.md) can create one for you.</span></span>

<span data-ttu-id="b3c80-114">Tento kurz vyžaduje hello Azure CLI verze verze 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="b3c80-114">This tutorial requires hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="b3c80-115">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="b3c80-115">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="b3c80-116">Pokud potřebujete tooupgrade, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="b3c80-116">If you need tooupgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="load-balancing-overview"></a><span data-ttu-id="b3c80-117">Přehled Vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="b3c80-117">Load balancing overview</span></span>

<span data-ttu-id="b3c80-118">Existují dvě vrstvy Vyrovnávání zatížení v clusteru služby Azure Container Service DC/OS:</span><span class="sxs-lookup"><span data-stu-id="b3c80-118">There are two load-balancing layers in an Azure Container Service DC/OS cluster:</span></span> 

<span data-ttu-id="b3c80-119">**Azure nástroj pro vyrovnávání zatížení** poskytuje veřejné vstupní body (ty, které této koncoví uživatelé přístup hello).</span><span class="sxs-lookup"><span data-stu-id="b3c80-119">**Azure Load Balancer** provides public entry points (hello ones that end users access).</span></span> <span data-ttu-id="b3c80-120">Azure LB automaticky poskytuje Azure Container Service a je ve výchozím nastavení nakonfigurované tooexpose port 80 a 443 8080.</span><span class="sxs-lookup"><span data-stu-id="b3c80-120">An Azure LB is provided automatically by Azure Container Service and is, by default, configured tooexpose port 80, 443 and 8080.</span></span>

<span data-ttu-id="b3c80-121">**Hello Marathon nástroj pro vyrovnávání zatížení (marathon-lb)** trasy příchozí požadavky toocontainer instancí, které tyto žádosti o služby.</span><span class="sxs-lookup"><span data-stu-id="b3c80-121">**hello Marathon Load Balancer (marathon-lb)** routes inbound requests toocontainer instances that service these requests.</span></span> <span data-ttu-id="b3c80-122">Škály hello kontejnerů, které poskytují naše webové služby, hello marathon-lb dynamicky přizpůsobení.</span><span class="sxs-lookup"><span data-stu-id="b3c80-122">As we scale hello containers providing our web service, hello marathon-lb dynamically adapts.</span></span> <span data-ttu-id="b3c80-123">Tento nástroj pro vyrovnávání zatížení není dostupné ve výchozím nastavení v kontejneru služby, ale je snadno tooinstall.</span><span class="sxs-lookup"><span data-stu-id="b3c80-123">This load balancer is not provided by default in your Container Service, but it is easy tooinstall.</span></span>

## <a name="configure-marathon-load-balancer"></a><span data-ttu-id="b3c80-124">Konfigurace pro vyrovnávání zatížení Marathon</span><span class="sxs-lookup"><span data-stu-id="b3c80-124">Configure Marathon Load Balancer</span></span>

<span data-ttu-id="b3c80-125">Nástroj pro vyrovnávání zatížení Marathon dynamicky změní konfiguraci založena na hello kontejnery, které jste nasadili.</span><span class="sxs-lookup"><span data-stu-id="b3c80-125">Marathon Load Balancer dynamically reconfigures itself based on hello containers that you've deployed.</span></span> <span data-ttu-id="b3c80-126">Je také odolný toohello ztrátě kontejneru nebo agenta – Pokud k tomu dojde, restartuje hello kontejner na jiném místě Apache Mesos a marathon-lb přizpůsobení.</span><span class="sxs-lookup"><span data-stu-id="b3c80-126">It's also resilient toohello loss of a container or an agent - if this occurs, Apache Mesos restarts hello container elsewhere and marathon-lb adapts.</span></span>

<span data-ttu-id="b3c80-127">Spusťte následující příkaz tooinstall hello marathon nástroj pro vyrovnávání zatížení v clusteru hello veřejného agenta hello.</span><span class="sxs-lookup"><span data-stu-id="b3c80-127">Run hello following command tooinstall hello marathon load balancer on hello public agent's cluster.</span></span>

```azurecli-interactive
dcos package install marathon-lb
```

## <a name="deploy-load-balanced-application"></a><span data-ttu-id="b3c80-128">Nasazení aplikace skupinu s vyrovnáváním zatížení</span><span class="sxs-lookup"><span data-stu-id="b3c80-128">Deploy load balanced application</span></span>

<span data-ttu-id="b3c80-129">Teď, když máme balíček marathon-lb hello, můžeme nasadit kontejner aplikace, že nám chcete vyrovnávat tooload.</span><span class="sxs-lookup"><span data-stu-id="b3c80-129">Now that we have hello marathon-lb package, we can deploy an application container that we wish tooload balance.</span></span> 

<span data-ttu-id="b3c80-130">Nejdřív získáte hello plně kvalifikovaný název domény hello veřejně vystaven agentů.</span><span class="sxs-lookup"><span data-stu-id="b3c80-130">First, get hello FQDN of hello publicly exposed agents.</span></span>

```azurecli-interactive
az acs list --resource-group myResourceGroup --query "[0].agentPoolProfiles[0].fqdn" --output tsv
```

<span data-ttu-id="b3c80-131">Dále vytvořte soubor s názvem *hello web.json* a kopírování v hello následující obsah.</span><span class="sxs-lookup"><span data-stu-id="b3c80-131">Next, create a file named *hello-web.json* and copy in hello following contents.</span></span> <span data-ttu-id="b3c80-132">Hello `HAPROXY_0_VHOST` popisku musí toobe aktualizováno hello plně kvalifikovaný název domény hello agentů DC/OS.</span><span class="sxs-lookup"><span data-stu-id="b3c80-132">hello `HAPROXY_0_VHOST` label needs toobe updated with hello FQDN of hello DC/OS agents.</span></span> 

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

<span data-ttu-id="b3c80-133">Hello rozhraní příkazového řádku DC/OS toorun hello aplikaci použijte.</span><span class="sxs-lookup"><span data-stu-id="b3c80-133">Use hello DC/OS CLI toorun hello application.</span></span> <span data-ttu-id="b3c80-134">Ve výchozím nastavení nasadí Marathon hello hello aplikace toohello privátní clusteru.</span><span class="sxs-lookup"><span data-stu-id="b3c80-134">By default Marathon deploys hello hello applicaton toohello private cluster.</span></span> <span data-ttu-id="b3c80-135">To znamená, že hello výše nasazení je pouze přístupné přes nástroj pro vyrovnávání zatížení, která je obvykle hello požadovaného chování.</span><span class="sxs-lookup"><span data-stu-id="b3c80-135">This means that hello above deployment is only accessible via your load balancer, which is usually hello desired behavior.</span></span>

```azurecli-interactive
dcos marathon app add hello-web.json
```

<span data-ttu-id="b3c80-136">Jakmile aplikace hello nasazen, procházejte toohello plně kvalifikovaný název domény aplikace hello agenta clusteru tooview skupinu s vyrovnáváním zatížení.</span><span class="sxs-lookup"><span data-stu-id="b3c80-136">Once hello application has been deployed, browse toohello FQDN of hello agent cluster tooview load balanced application.</span></span>

![Bitové kopie aplikace skupinu s vyrovnáváním zatížení](./media/container-service-load-balancing/lb-app.png)

## <a name="configure-azure-load-balancer"></a><span data-ttu-id="b3c80-138">Konfigurace pro vyrovnávání zatížení Azure</span><span class="sxs-lookup"><span data-stu-id="b3c80-138">Configure Azure Load Balancer</span></span>

<span data-ttu-id="b3c80-139">Služba Azure Load Balancer ve výchozím nastavení zpřístupňuje porty 80, 8080 a 443.</span><span class="sxs-lookup"><span data-stu-id="b3c80-139">By default, Azure Load Balancer exposes ports 80, 8080, and 443.</span></span> <span data-ttu-id="b3c80-140">Pokud používáte některý z těchto tří portů (jako My v hello výše příkladu), pak není co že budete potřebovat toodo.</span><span class="sxs-lookup"><span data-stu-id="b3c80-140">If you're using one of these three ports (as we do in hello above example), then there is nothing you need toodo.</span></span> <span data-ttu-id="b3c80-141">Musí být schopný toohit plně kvalifikovaný název domény služby Vyrovnávání zatížení agenta a pokaždé, když aktualizujete, můžete budete použít jeden ze tří webových serverů v kruhového dotazování.</span><span class="sxs-lookup"><span data-stu-id="b3c80-141">You should be able toohit your agent load balancer's FQDN, and each time you refresh, you'll hit one of your three web servers in a round-robin fashion.</span></span> 

<span data-ttu-id="b3c80-142">Pokud používáte jiný port, musíte pravidlo tooadd kruhového dotazování a kontroly na Vyrovnávání zatížení hello hello portu, který jste použili pro.</span><span class="sxs-lookup"><span data-stu-id="b3c80-142">If you use a different port, you need tooadd a round-robin rule and a probe on hello load balancer for hello port that you used.</span></span> <span data-ttu-id="b3c80-143">Můžete to provést z hello [rozhraní příkazového řádku Azure](../../azure-resource-manager/xplat-cli-azure-resource-manager.md), pomocí příkazů hello `azure network lb rule create` a `azure network lb probe create`.</span><span class="sxs-lookup"><span data-stu-id="b3c80-143">You can do this from hello [Azure CLI](../../azure-resource-manager/xplat-cli-azure-resource-manager.md), with hello commands `azure network lb rule create` and `azure network lb probe create`.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b3c80-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b3c80-144">Next steps</span></span>

<span data-ttu-id="b3c80-145">V tomto kurzu jste se dozvěděli o vyrovnávání zatížení v ACS s hello Marathon i zatížení Azure včetně služby Vyrovnávání hello následující akce:</span><span class="sxs-lookup"><span data-stu-id="b3c80-145">In this tutorial, you learned about load balancing in ACS with both hello Marathon and Azure load balancers including hello following actions:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b3c80-146">Konfigurace služby Vyrovnávání zatížení Marathon</span><span class="sxs-lookup"><span data-stu-id="b3c80-146">Configure a Marathon Load Balancer</span></span>
> * <span data-ttu-id="b3c80-147">Nasazení aplikace pomocí nástroje pro vyrovnávání zatížení hello</span><span class="sxs-lookup"><span data-stu-id="b3c80-147">Deploy an application using hello load balancer</span></span>
> * <span data-ttu-id="b3c80-148">Konfigurace a nástroj pro vyrovnávání zatížení Azure</span><span class="sxs-lookup"><span data-stu-id="b3c80-148">Configure and Azure load balancer</span></span>

<span data-ttu-id="b3c80-149">Posunutí další kurz toolearn toohello o integraci Azure storage s DC/OS v Azure.</span><span class="sxs-lookup"><span data-stu-id="b3c80-149">Advance toohello next tutorial toolearn about integrating Azure storage with DC/OS in Azure.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="b3c80-150">Azure připojit sdílenou složku v clusteru DC/OS</span><span class="sxs-lookup"><span data-stu-id="b3c80-150">Mount Azure File Share in DC/OS cluster</span></span>](container-service-dcos-fileshare.md)