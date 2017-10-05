---
title: "Správě Azure Kubernetes clusteru pomocí webového uživatelského rozhraní | Microsoft Docs"
description: "V Azure Container Service pomocí Kubernetes webového uživatelského rozhraní"
services: container-service
documentationcenter: 
author: bburns
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/21/2017
ms.author: bburns
ms.custom: mvc
ms.openlocfilehash: e31f90d61fc61f17582372fe9f491a1e21f628b0
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="using-the-kubernetes-web-ui-with-azure-container-service"></a><span data-ttu-id="8cc16-103">Pomocí Azure Container Service Kubernetes webového uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="8cc16-103">Using the Kubernetes web UI with Azure Container Service</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8cc16-104">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8cc16-104">Prerequisites</span></span>
<span data-ttu-id="8cc16-105">Tento návod předpokládá, že máte [vytvořit Kubernetes clusteru Azure Container Service pomocí](container-service-kubernetes-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="8cc16-105">This walkthrough assumes that you have [created a Kubernetes cluster using Azure Container Service](container-service-kubernetes-walkthrough.md).</span></span>


<span data-ttu-id="8cc16-106">Také předpokládá, že máte Azure CLI 2.0 a `kubectl` nástroje nainstalované.</span><span class="sxs-lookup"><span data-stu-id="8cc16-106">It also assumes that you have the Azure CLI 2.0 and `kubectl` tools installed.</span></span>

<span data-ttu-id="8cc16-107">Můžete otestovat, pokud máte `az` nainstalovaná, spuštěním nástroje:</span><span class="sxs-lookup"><span data-stu-id="8cc16-107">You can test if you have the `az` tool installed by running:</span></span>

```console
$ az --version
```

<span data-ttu-id="8cc16-108">Pokud nemáte `az` nástroj nainstalovali, jsou k dispozici pokyny [zde](https://github.com/azure/azure-cli#installation).</span><span class="sxs-lookup"><span data-stu-id="8cc16-108">If you don't have the `az` tool installed, there are instructions [here](https://github.com/azure/azure-cli#installation).</span></span>

<span data-ttu-id="8cc16-109">Můžete otestovat, pokud máte `kubectl` nainstalovaná, spuštěním nástroje:</span><span class="sxs-lookup"><span data-stu-id="8cc16-109">You can test if you have the `kubectl` tool installed by running:</span></span>

```console
$ kubectl version
```

<span data-ttu-id="8cc16-110">Pokud nemáte `kubectl` nainstalován, můžete spustit:</span><span class="sxs-lookup"><span data-stu-id="8cc16-110">If you don't have `kubectl` installed, you can run:</span></span>

```console
$ az acs kubernetes install-cli
```

## <a name="overview"></a><span data-ttu-id="8cc16-111">Přehled</span><span class="sxs-lookup"><span data-stu-id="8cc16-111">Overview</span></span>

### <a name="connect-to-the-web-ui"></a><span data-ttu-id="8cc16-112">Připojit k serveru webového uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="8cc16-112">Connect to the web UI</span></span>
<span data-ttu-id="8cc16-113">Spuštěním můžete spustit Kubernetes webového uživatelského rozhraní:</span><span class="sxs-lookup"><span data-stu-id="8cc16-113">You can launch the Kubernetes web UI by running:</span></span>

```console
$ az acs kubernetes browse -g [Resource Group] -n [Container service instance name]
```

<span data-ttu-id="8cc16-114">To by měla otevřít webový prohlížeč nakonfigurován tak, aby obraťte se na proxy server zabezpečené připojení místního počítače k Kubernetes webového uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="8cc16-114">This should open a web browser configured to talk to a secure proxy connecting your local machine to the Kubernetes web UI.</span></span>

### <a name="create-and-expose-a-service"></a><span data-ttu-id="8cc16-115">Vytvoření a vystavení služby</span><span class="sxs-lookup"><span data-stu-id="8cc16-115">Create and expose a service</span></span>
1. <span data-ttu-id="8cc16-116">V Kubernetes webového uživatelského rozhraní, klikněte na **vytvořit** tlačítko v horním pravém okně.</span><span class="sxs-lookup"><span data-stu-id="8cc16-116">In the Kubernetes web UI, click **Create** button in the upper right window.</span></span>

    ![Kubernetes vytvoření uživatelského rozhraní](./media/container-service-kubernetes-ui/create.png)

    <span data-ttu-id="8cc16-118">Otevře se dialogové okno kde můžete začít vytvářet aplikace.</span><span class="sxs-lookup"><span data-stu-id="8cc16-118">A dialog box opens where you can start creating your application.</span></span>

2. <span data-ttu-id="8cc16-119">Poskytněte název `hello-nginx`.</span><span class="sxs-lookup"><span data-stu-id="8cc16-119">Give it the name `hello-nginx`.</span></span> <span data-ttu-id="8cc16-120">Použití [ `nginx` kontejneru z Docker](https://hub.docker.com/_/nginx/) a nasadit tři repliky této webové služby.</span><span class="sxs-lookup"><span data-stu-id="8cc16-120">Use the [`nginx` container from Docker](https://hub.docker.com/_/nginx/) and deploy three replicas of this web service.</span></span>

    ![Dialogové okno Vytvořit Kubernetes Pod](./media/container-service-kubernetes-ui/nginx.png)

3. <span data-ttu-id="8cc16-122">V části **služby**, vyberte **externí** a zadejte port 80.</span><span class="sxs-lookup"><span data-stu-id="8cc16-122">Under **Service**, select **External** and enter port 80.</span></span>

    <span data-ttu-id="8cc16-123">Toto nastavení zatížení zůstatky provoz na tři repliky.</span><span class="sxs-lookup"><span data-stu-id="8cc16-123">This setting load-balances traffic to the three replicas.</span></span>

    ![Dialogové okno Vytvořit Kubernetes služby](./media/container-service-kubernetes-ui/service.png)

4. <span data-ttu-id="8cc16-125">Klikněte na tlačítko **nasadit** k nasazení těchto kontejnerů a služeb.</span><span class="sxs-lookup"><span data-stu-id="8cc16-125">Click **Deploy** to deploy these containers and services.</span></span>

    ![Kubernetes nasazení](./media/container-service-kubernetes-ui/deploy.png)

### <a name="view-your-containers"></a><span data-ttu-id="8cc16-127">Zobrazit vaše kontejnery</span><span class="sxs-lookup"><span data-stu-id="8cc16-127">View your containers</span></span>
<span data-ttu-id="8cc16-128">Po kliknutí na tlačítko **nasadit**, uživatelské rozhraní zobrazí zobrazení vaší služby jako nasazení:</span><span class="sxs-lookup"><span data-stu-id="8cc16-128">After you click **Deploy**, the UI shows a view of your service as it deploys:</span></span>

![Stav Kubernetes](./media/container-service-kubernetes-ui/status.png)

<span data-ttu-id="8cc16-130">Stav každého objektu Kubernetes na zobrazený na levé straně uživatelského rozhraní, můžete zobrazit v části **pracovními stanicemi soustředěnými kolem**.</span><span class="sxs-lookup"><span data-stu-id="8cc16-130">You can see the status of each Kubernetes object in the circle on the left-hand side of the UI, under **Pods**.</span></span> <span data-ttu-id="8cc16-131">Pokud je částečně úplné kruh, objekt stále nasazení.</span><span class="sxs-lookup"><span data-stu-id="8cc16-131">If it is a partially full circle, then the object is still deploying.</span></span> <span data-ttu-id="8cc16-132">Po nasazení objekt zobrazí zelená značka zaškrtnutí:</span><span class="sxs-lookup"><span data-stu-id="8cc16-132">When an object is fully deployed, it displays a green check mark:</span></span>

![Kubernetes nasazení](./media/container-service-kubernetes-ui/deployed.png)

<span data-ttu-id="8cc16-134">Jakmile všechno běží, klikněte na jednu z vaší pracovními stanicemi soustředěnými kolem zobrazíte podrobnosti o webové službě spuštěné.</span><span class="sxs-lookup"><span data-stu-id="8cc16-134">Once everything is running, click one of your pods to see details about the running web service.</span></span>

![Kubernetes pracovními stanicemi soustředěnými kolem](./media/container-service-kubernetes-ui/pods.png)

<span data-ttu-id="8cc16-136">V **pracovními stanicemi soustředěnými kolem** zobrazení, se zobrazí informace o kontejnery v pod, jakož i prostředky procesoru a paměti používané těchto kontejnerů:</span><span class="sxs-lookup"><span data-stu-id="8cc16-136">In the **Pods** view, you can see information about the containers in the pod as well as the CPU and memory resources used by those containers:</span></span>

![Kubernetes prostředky](./media/container-service-kubernetes-ui/resources.png)

<span data-ttu-id="8cc16-138">Pokud nevidíte prostředky, musíte Počkejte několik minut pro data sledování rozšíření.</span><span class="sxs-lookup"><span data-stu-id="8cc16-138">If you don't see the resources, you may need to wait a few minutes for the monitoring data to propagate.</span></span>

<span data-ttu-id="8cc16-139">Chcete-li zobrazit protokoly pro váš kontejner, klikněte na tlačítko **zobrazit protokoly**.</span><span class="sxs-lookup"><span data-stu-id="8cc16-139">To see the logs for your container, click **View logs**.</span></span>

![Protokoly Kubernetes](./media/container-service-kubernetes-ui/logs.png)

### <a name="viewing-your-service"></a><span data-ttu-id="8cc16-141">Zobrazení služby</span><span class="sxs-lookup"><span data-stu-id="8cc16-141">Viewing your service</span></span>
<span data-ttu-id="8cc16-142">Kromě spuštění kontejnerů, rozhraní Kubernetes vytvořil externí `Service` která zřizuje Vyrovnávání zatížení, aby provoz do kontejnerů v clusteru.</span><span class="sxs-lookup"><span data-stu-id="8cc16-142">In addition to running your containers, the Kubernetes UI has created an external `Service` which provisions a load balancer to bring traffic to the containers in your cluster.</span></span>

<span data-ttu-id="8cc16-143">V levém navigačním podokně klikněte na tlačítko **služby** zobrazíte všechny služby (měla by existovat pouze jedna).</span><span class="sxs-lookup"><span data-stu-id="8cc16-143">In the left navigation pane, click **Services** to view all services (there should be only one).</span></span>

![Kubernetes služby](./media/container-service-kubernetes-ui/service-deployed.png)

<span data-ttu-id="8cc16-145">V tomto zobrazení měli byste vidět externí koncový bod (IP adresa) přiřazené k službě.</span><span class="sxs-lookup"><span data-stu-id="8cc16-145">In that view, you should see an external endpoint (IP address) that has been allocated to your service.</span></span>
<span data-ttu-id="8cc16-146">Pokud kliknete na tuto IP adresu, měli byste vidět vaše kontejner nginx a sváže s za nástrojem pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="8cc16-146">If you click that IP address, you should see your Nginx container running behind the load balancer.</span></span>

![nginx zobrazení](./media/container-service-kubernetes-ui/nginx-page.png)

### <a name="resizing-your-service"></a><span data-ttu-id="8cc16-148">Změna velikosti služby</span><span class="sxs-lookup"><span data-stu-id="8cc16-148">Resizing your service</span></span>
<span data-ttu-id="8cc16-149">Kromě zobrazování vašich objektů v uživatelském rozhraní, můžete upravit a aktualizovat objekty Kubernetes rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="8cc16-149">In addition to viewing your objects in the UI, you can edit and update the Kubernetes API objects.</span></span>

<span data-ttu-id="8cc16-150">Nejprve, klikněte na tlačítko **nasazení** v levém navigačním podokně zobrazíte nasazení pro vaši službu.</span><span class="sxs-lookup"><span data-stu-id="8cc16-150">First, click **Deployments** in the left navigation pane to see the deployment for your service.</span></span>

<span data-ttu-id="8cc16-151">Až se v tomto zobrazení, klikněte na sady replik a pak klikněte na tlačítko **upravit** v horním navigačním panelu:</span><span class="sxs-lookup"><span data-stu-id="8cc16-151">Once you are in that view, click on the replica set, and then click **Edit** in the upper navigation bar:</span></span>

![Upravit Kubernetes](./media/container-service-kubernetes-ui/edit.png)

<span data-ttu-id="8cc16-153">Upravit `spec.replicas` pole, které chcete být `2`a klikněte na tlačítko **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="8cc16-153">Edit the `spec.replicas` field to be `2`, and click **Update**.</span></span>

<span data-ttu-id="8cc16-154">To způsobí, že počet replik odpojení ke dvěma odstraněním vaší pracovními stanicemi soustředěnými kolem.</span><span class="sxs-lookup"><span data-stu-id="8cc16-154">This causes the number of replicas to drop to two by deleting one of your pods.</span></span>

 

