---
title: "aaaManage Azure Kubernetes cluster s webového uživatelského rozhraní | Microsoft Docs"
description: "Pomocí hello Kubernetes webové uživatelské rozhraní v Azure Container Service"
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
ms.openlocfilehash: e24ea0b82c94d2fd4610e4442699ef756590e6bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-kubernetes-web-ui-with-azure-container-service"></a><span data-ttu-id="6252f-103">Pomocí hello Kubernetes webové uživatelské rozhraní s Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="6252f-103">Using hello Kubernetes web UI with Azure Container Service</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6252f-104">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6252f-104">Prerequisites</span></span>
<span data-ttu-id="6252f-105">Tento návod předpokládá, že máte [vytvořit Kubernetes clusteru Azure Container Service pomocí](container-service-kubernetes-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="6252f-105">This walkthrough assumes that you have [created a Kubernetes cluster using Azure Container Service](container-service-kubernetes-walkthrough.md).</span></span>


<span data-ttu-id="6252f-106">Také předpokládá, že máte hello Azure CLI 2.0 a `kubectl` nástroje nainstalované.</span><span class="sxs-lookup"><span data-stu-id="6252f-106">It also assumes that you have hello Azure CLI 2.0 and `kubectl` tools installed.</span></span>

<span data-ttu-id="6252f-107">Pokud máte hello můžete otestovat `az` nainstalovaná, spuštěním nástroje:</span><span class="sxs-lookup"><span data-stu-id="6252f-107">You can test if you have hello `az` tool installed by running:</span></span>

```console
$ az --version
```

<span data-ttu-id="6252f-108">Pokud nemáte hello `az` nástroj nainstalovali, jsou k dispozici pokyny [zde](https://github.com/azure/azure-cli#installation).</span><span class="sxs-lookup"><span data-stu-id="6252f-108">If you don't have hello `az` tool installed, there are instructions [here](https://github.com/azure/azure-cli#installation).</span></span>

<span data-ttu-id="6252f-109">Pokud máte hello můžete otestovat `kubectl` nainstalovaná, spuštěním nástroje:</span><span class="sxs-lookup"><span data-stu-id="6252f-109">You can test if you have hello `kubectl` tool installed by running:</span></span>

```console
$ kubectl version
```

<span data-ttu-id="6252f-110">Pokud nemáte `kubectl` nainstalován, můžete spustit:</span><span class="sxs-lookup"><span data-stu-id="6252f-110">If you don't have `kubectl` installed, you can run:</span></span>

```console
$ az acs kubernetes install-cli
```

## <a name="overview"></a><span data-ttu-id="6252f-111">Přehled</span><span class="sxs-lookup"><span data-stu-id="6252f-111">Overview</span></span>

### <a name="connect-toohello-web-ui"></a><span data-ttu-id="6252f-112">Připojit toohello webového uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="6252f-112">Connect toohello web UI</span></span>
<span data-ttu-id="6252f-113">Spuštěním můžete spustit hello Kubernetes webového uživatelského rozhraní:</span><span class="sxs-lookup"><span data-stu-id="6252f-113">You can launch hello Kubernetes web UI by running:</span></span>

```console
$ az acs kubernetes browse -g [Resource Group] -n [Container service instance name]
```

<span data-ttu-id="6252f-114">To by měla otevřít webový prohlížeč nakonfigurován tootalk tooa zabezpečené proxy server připojení místního počítače toohello Kubernetes webového uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="6252f-114">This should open a web browser configured tootalk tooa secure proxy connecting your local machine toohello Kubernetes web UI.</span></span>

### <a name="create-and-expose-a-service"></a><span data-ttu-id="6252f-115">Vytvoření a vystavení služby</span><span class="sxs-lookup"><span data-stu-id="6252f-115">Create and expose a service</span></span>
1. <span data-ttu-id="6252f-116">V hello Kubernetes webového uživatelského rozhraní, klikněte na **vytvořit** tlačítko v pravém horním hello.</span><span class="sxs-lookup"><span data-stu-id="6252f-116">In hello Kubernetes web UI, click **Create** button in hello upper right window.</span></span>

    ![Kubernetes vytvoření uživatelského rozhraní](./media/container-service-kubernetes-ui/create.png)

    <span data-ttu-id="6252f-118">Otevře se dialogové okno kde můžete začít vytvářet aplikace.</span><span class="sxs-lookup"><span data-stu-id="6252f-118">A dialog box opens where you can start creating your application.</span></span>

2. <span data-ttu-id="6252f-119">Pojmenujte ji hello `hello-nginx`.</span><span class="sxs-lookup"><span data-stu-id="6252f-119">Give it hello name `hello-nginx`.</span></span> <span data-ttu-id="6252f-120">Použití hello [ `nginx` kontejneru z Docker](https://hub.docker.com/_/nginx/) a nasadit tři repliky této webové služby.</span><span class="sxs-lookup"><span data-stu-id="6252f-120">Use hello [`nginx` container from Docker](https://hub.docker.com/_/nginx/) and deploy three replicas of this web service.</span></span>

    ![Dialogové okno Vytvořit Kubernetes Pod](./media/container-service-kubernetes-ui/nginx.png)

3. <span data-ttu-id="6252f-122">V části **služby**, vyberte **externí** a zadejte port 80.</span><span class="sxs-lookup"><span data-stu-id="6252f-122">Under **Service**, select **External** and enter port 80.</span></span>

    <span data-ttu-id="6252f-123">Toto nastavení zatížení zůstatky provoz toohello tři repliky.</span><span class="sxs-lookup"><span data-stu-id="6252f-123">This setting load-balances traffic toohello three replicas.</span></span>

    ![Dialogové okno Vytvořit Kubernetes služby](./media/container-service-kubernetes-ui/service.png)

4. <span data-ttu-id="6252f-125">Klikněte na tlačítko **nasadit** toodeploy tyto kontejnery a služby.</span><span class="sxs-lookup"><span data-stu-id="6252f-125">Click **Deploy** toodeploy these containers and services.</span></span>

    ![Kubernetes nasazení](./media/container-service-kubernetes-ui/deploy.png)

### <a name="view-your-containers"></a><span data-ttu-id="6252f-127">Zobrazit vaše kontejnery</span><span class="sxs-lookup"><span data-stu-id="6252f-127">View your containers</span></span>
<span data-ttu-id="6252f-128">Po kliknutí na tlačítko **nasadit**, hello uživatelského rozhraní ukazuje zobrazení vaší služby jako nasazení:</span><span class="sxs-lookup"><span data-stu-id="6252f-128">After you click **Deploy**, hello UI shows a view of your service as it deploys:</span></span>

![Stav Kubernetes](./media/container-service-kubernetes-ui/status.png)

<span data-ttu-id="6252f-130">Hello stav každého objektu Kubernetes v kruhu hello na levé straně hello uživatelského rozhraní, můžete zobrazit v části **pracovními stanicemi soustředěnými kolem**.</span><span class="sxs-lookup"><span data-stu-id="6252f-130">You can see hello status of each Kubernetes object in hello circle on hello left-hand side of the UI, under **Pods**.</span></span> <span data-ttu-id="6252f-131">Pokud je částečně úplné kruh, objekt hello stále nasazení.</span><span class="sxs-lookup"><span data-stu-id="6252f-131">If it is a partially full circle, then hello object is still deploying.</span></span> <span data-ttu-id="6252f-132">Po nasazení objekt zobrazí zelená značka zaškrtnutí:</span><span class="sxs-lookup"><span data-stu-id="6252f-132">When an object is fully deployed, it displays a green check mark:</span></span>

![Kubernetes nasazení](./media/container-service-kubernetes-ui/deployed.png)

<span data-ttu-id="6252f-134">Jakmile všechno běží, klikněte na jednu z vaší pracovními stanicemi soustředěnými kolem toosee podrobnosti o hello spuštění webové služby.</span><span class="sxs-lookup"><span data-stu-id="6252f-134">Once everything is running, click one of your pods toosee details about hello running web service.</span></span>

![Kubernetes pracovními stanicemi soustředěnými kolem](./media/container-service-kubernetes-ui/pods.png)

<span data-ttu-id="6252f-136">V hello **pracovními stanicemi soustředěnými kolem** zobrazení, se zobrazí informace o hello kontejnery v hello pod, jakož i prostředků procesoru a paměti hello používá těchto kontejnerů:</span><span class="sxs-lookup"><span data-stu-id="6252f-136">In hello **Pods** view, you can see information about hello containers in hello pod as well as hello CPU and memory resources used by those containers:</span></span>

![Kubernetes prostředky](./media/container-service-kubernetes-ui/resources.png)

<span data-ttu-id="6252f-138">Pokud nevidíte hello prostředky, může pro hello monitorování toopropagate dat potřebovat toowait několik minut.</span><span class="sxs-lookup"><span data-stu-id="6252f-138">If you don't see hello resources, you may need toowait a few minutes for hello monitoring data toopropagate.</span></span>

<span data-ttu-id="6252f-139">toosee hello protokoly pro kontejner, klikněte na tlačítko **zobrazit protokoly**.</span><span class="sxs-lookup"><span data-stu-id="6252f-139">toosee hello logs for your container, click **View logs**.</span></span>

![Protokoly Kubernetes](./media/container-service-kubernetes-ui/logs.png)

### <a name="viewing-your-service"></a><span data-ttu-id="6252f-141">Zobrazení služby</span><span class="sxs-lookup"><span data-stu-id="6252f-141">Viewing your service</span></span>
<span data-ttu-id="6252f-142">V přidání toorunning kontejnerů, hello uživatelského rozhraní Kubernetes vytvořil externí `Service` která zřizuje zatížení vyrovnávání toobring provoz toohello kontejnery v clusteru.</span><span class="sxs-lookup"><span data-stu-id="6252f-142">In addition toorunning your containers, hello Kubernetes UI has created an external `Service` which provisions a load balancer toobring traffic toohello containers in your cluster.</span></span>

<span data-ttu-id="6252f-143">V levém navigačním podokně hello, klikněte na **služby** tooview všechny služby (měla by existovat pouze jedna).</span><span class="sxs-lookup"><span data-stu-id="6252f-143">In hello left navigation pane, click **Services** tooview all services (there should be only one).</span></span>

![Kubernetes služby](./media/container-service-kubernetes-ui/service-deployed.png)

<span data-ttu-id="6252f-145">V tomto zobrazení měli byste vidět externí koncový bod (IP adresa) přiřazené tooyour služby.</span><span class="sxs-lookup"><span data-stu-id="6252f-145">In that view, you should see an external endpoint (IP address) that has been allocated tooyour service.</span></span>
<span data-ttu-id="6252f-146">Pokud kliknete na tuto IP adresu, měli byste vidět vaše kontejner nginx a sváže s za nástrojem pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="6252f-146">If you click that IP address, you should see your Nginx container running behind the load balancer.</span></span>

![nginx zobrazení](./media/container-service-kubernetes-ui/nginx-page.png)

### <a name="resizing-your-service"></a><span data-ttu-id="6252f-148">Změna velikosti služby</span><span class="sxs-lookup"><span data-stu-id="6252f-148">Resizing your service</span></span>
<span data-ttu-id="6252f-149">V přidání tooviewing vašich objektů v hello uživatelského rozhraní, můžete upravit a aktualizovat objekty rozhraní API Kubernetes hello.</span><span class="sxs-lookup"><span data-stu-id="6252f-149">In addition tooviewing your objects in hello UI, you can edit and update hello Kubernetes API objects.</span></span>

<span data-ttu-id="6252f-150">Nejprve, klikněte na tlačítko **nasazení** v hello levé navigační podokně toosee hello nasazení pro vaši službu.</span><span class="sxs-lookup"><span data-stu-id="6252f-150">First, click **Deployments** in hello left navigation pane toosee hello deployment for your service.</span></span>

<span data-ttu-id="6252f-151">Až se v tomto zobrazení, klikněte na hello sady replik a pak klikněte na tlačítko **upravit** v horním navigačním panelu hello:</span><span class="sxs-lookup"><span data-stu-id="6252f-151">Once you are in that view, click on hello replica set, and then click **Edit** in hello upper navigation bar:</span></span>

![Upravit Kubernetes](./media/container-service-kubernetes-ui/edit.png)

<span data-ttu-id="6252f-153">Upravit hello `spec.replicas` pole toobe `2`a klikněte na tlačítko **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="6252f-153">Edit hello `spec.replicas` field toobe `2`, and click **Update**.</span></span>

<span data-ttu-id="6252f-154">To způsobí, že hello počet replik toodrop tootwo odstraněním vaší pracovními stanicemi soustředěnými kolem.</span><span class="sxs-lookup"><span data-stu-id="6252f-154">This causes hello number of replicas toodrop tootwo by deleting one of your pods.</span></span>

 

