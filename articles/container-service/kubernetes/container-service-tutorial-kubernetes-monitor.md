---
title: "kurz pro službu kontejneru aaaAzure – Kubernetes monitorování | Microsoft Docs"
description: "Kurz pro Azure Container Service – monitorování Kubernetes s Operations Management Suite (OMS)"
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: Docker, kontejnery, Micro-services, Kubernetes, Azure
ms.assetid: 
ms.service: container-service
ms.devlang: aurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: 54fa789453768529deaf25d7575e5b21d0e41882
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-a-kubernetes-cluster-with-operations-management-suite"></a><span data-ttu-id="99baa-104">Monitorování Kubernetes clusteru se službou Operations Management Suite</span><span class="sxs-lookup"><span data-stu-id="99baa-104">Monitor a Kubernetes cluster with Operations Management Suite</span></span>

<span data-ttu-id="99baa-105">Monitorování Kubernetes clusteru a kontejnerů, je důležité, zejména v případě, že spravujete provozní cluster ve velkém měřítku s více aplikacemi.</span><span class="sxs-lookup"><span data-stu-id="99baa-105">Monitoring your Kubernetes cluster and containers is critical, especially when you manage a production cluster at scale with multiple apps.</span></span> 

<span data-ttu-id="99baa-106">Můžete využít výhod několik Kubernetes řešení monitorování, buď od společnosti Microsoft nebo jiní poskytovatelé.</span><span class="sxs-lookup"><span data-stu-id="99baa-106">You can take advantage of several Kubernetes monitoring solutions, either from Microsoft or other providers.</span></span> <span data-ttu-id="99baa-107">V tomto kurzu můžete monitorovat Kubernetes clusteru pomocí řešení hello kontejnery v [Operations Management Suite](../../operations-management-suite/operations-management-suite-overview.md), řešení správy založené na cloudu IT společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="99baa-107">In this tutorial, you monitor your Kubernetes cluster by using hello Containers solution in [Operations Management Suite](../../operations-management-suite/operations-management-suite-overview.md), Microsoft's cloud-based IT management solution.</span></span> <span data-ttu-id="99baa-108">(hello OMS kontejnery řešení je ve verzi preview.)</span><span class="sxs-lookup"><span data-stu-id="99baa-108">(hello OMS Containers solution is in preview.)</span></span>

<span data-ttu-id="99baa-109">V tomto kurzu součástí sedm 7, obsahuje hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="99baa-109">This tutorial, part seven of seven, covers hello following tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="99baa-110">Získat nastavení pracovní prostor OMS</span><span class="sxs-lookup"><span data-stu-id="99baa-110">Get OMS Workspace settings</span></span>
> * <span data-ttu-id="99baa-111">Nastavit OMS agenty na uzly Kubernetes hello</span><span class="sxs-lookup"><span data-stu-id="99baa-111">Set up OMS agents on hello Kubernetes nodes</span></span>
> * <span data-ttu-id="99baa-112">Přístup k monitorování informací hello OMS portál nebo portál Azure</span><span class="sxs-lookup"><span data-stu-id="99baa-112">Access monitoring information in hello OMS portal or Azure portal</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="99baa-113">Než začnete</span><span class="sxs-lookup"><span data-stu-id="99baa-113">Before you begin</span></span>

<span data-ttu-id="99baa-114">V předchozích kurzy aplikace se zabalí do kontejneru bitové kopie, těchto bitových kopií nahráli tooAzure registru kontejneru a cluster Kubernetes vytvořit.</span><span class="sxs-lookup"><span data-stu-id="99baa-114">In previous tutorials, an application was packaged into container images, these images uploaded tooAzure Container Registry, and a Kubernetes cluster created.</span></span> <span data-ttu-id="99baa-115">Pokud jste ještě provést tyto kroky a chcete toofollow společně, vrátí příliš[kurzu 1 – Vytvoření kontejneru image](./container-service-tutorial-kubernetes-prepare-app.md).</span><span class="sxs-lookup"><span data-stu-id="99baa-115">If you have not done these steps, and would like toofollow along, return too[Tutorial 1 – Create container images](./container-service-tutorial-kubernetes-prepare-app.md).</span></span> 

<span data-ttu-id="99baa-116">Minimálně tento kurz vyžaduje Kubernetes cluster s uzly agenta systému Linux a účet služby Operations Management Suite (OMS).</span><span class="sxs-lookup"><span data-stu-id="99baa-116">At minimum, this tutorial requires a Kubernetes cluster with Linux agent nodes, and an Operations Management Suite (OMS) account.</span></span> <span data-ttu-id="99baa-117">V případě potřeby si zaregistrovat [bezplatnou zkušební verzi OMS](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial).</span><span class="sxs-lookup"><span data-stu-id="99baa-117">If needed, sign up for a [free OMS trial](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial).</span></span>

## <a name="get-workspace-settings"></a><span data-ttu-id="99baa-118">Získat nastavení pracovního prostoru</span><span class="sxs-lookup"><span data-stu-id="99baa-118">Get Workspace settings</span></span>

<span data-ttu-id="99baa-119">Když dostanete hello [portálu OMS](https://mms.microsoft.com), přejděte příliš**nastavení** > **připojené zdroje** > **servery se systémem Linux**.</span><span class="sxs-lookup"><span data-stu-id="99baa-119">When you can access hello [OMS portal](https://mms.microsoft.com), go too**Settings** > **Connected Sources** > **Linux Servers**.</span></span> <span data-ttu-id="99baa-120">Zde můžete najít hello *ID pracovního prostoru* a primární nebo sekundární *klíč pracovního prostoru*.</span><span class="sxs-lookup"><span data-stu-id="99baa-120">There, you can find hello *Workspace ID* and a primary or secondary *Workspace Key*.</span></span> <span data-ttu-id="99baa-121">Poznamenejte si tyto hodnoty, které budete potřebovat tooset až OMS agenty na hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="99baa-121">Take note of these values, which you need tooset up OMS agents on hello cluster.</span></span>

## <a name="set-up-oms-agents"></a><span data-ttu-id="99baa-122">Nastavit OMS agentů</span><span class="sxs-lookup"><span data-stu-id="99baa-122">Set up OMS agents</span></span>

<span data-ttu-id="99baa-123">Zde je souboru tooset YAML až OMS agenty na uzly clusteru Linux hello.</span><span class="sxs-lookup"><span data-stu-id="99baa-123">Here is a YAML file tooset up OMS agents on hello Linux cluster nodes.</span></span> <span data-ttu-id="99baa-124">Vytvoří Kubernetes [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/), která se spouští jednu identické pod na každém uzlu clusteru.</span><span class="sxs-lookup"><span data-stu-id="99baa-124">It creates a Kubernetes [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/), which runs a single identical pod on each cluster node.</span></span> <span data-ttu-id="99baa-125">Hello DaemonSet prostředků je ideální pro nasazení agenta monitorování.</span><span class="sxs-lookup"><span data-stu-id="99baa-125">hello DaemonSet resource is ideal for deploying a monitoring agent.</span></span> 

<span data-ttu-id="99baa-126">Uložit hello následující text tooa soubor s názvem `oms-daemonset.yaml`a nahraďte zástupný symbol hodnoty hello *myWorkspaceID* a *myWorkspaceKey* s ID pracovního prostoru OMS a klíč.</span><span class="sxs-lookup"><span data-stu-id="99baa-126">Save hello following text tooa file named `oms-daemonset.yaml`, and replace hello placeholder values for *myWorkspaceID* and *myWorkspaceKey* with your OMS Workspace ID and Key.</span></span> <span data-ttu-id="99baa-127">(V produkčním prostředí, můžete zakódovat tyto hodnoty jako tajných klíčů.)</span><span class="sxs-lookup"><span data-stu-id="99baa-127">(In production, you can encode these values as secrets.)</span></span>

```YAML
apiVersion: extensions/v1beta1
kind: DaemonSet
metadata:
 name: omsagent
spec:
 template:
  metadata:
   labels:
    app: omsagent
    agentVersion: v1.3.4-127
    dockerProviderVersion: 10.0.0-25
  spec:
   containers:
     - name: omsagent 
       image: "microsoft/oms"
       imagePullPolicy: Always
       env:
       - name: WSID
         value: myWorkspaceID
       - name: KEY 
         value: myWorkspaceKey
       - name: DOMAIN
         value: opinsights.azure.com
       securityContext:
         privileged: true
       ports:
       - containerPort: 25225
         protocol: TCP 
       - containerPort: 25224
         protocol: UDP
       volumeMounts:
        - mountPath: /var/run/docker.sock
          name: docker-sock
        - mountPath: /var/log 
          name: host-log
       livenessProbe:
        exec:
         command:
         - /bin/bash
         - -c
         - ps -ef | grep omsagent | grep -v "grep"
        initialDelaySeconds: 60
        periodSeconds: 60
   volumes:
    - name: docker-sock 
      hostPath:
       path: /var/run/docker.sock
    - name: host-log
      hostPath:
       path: /var/log
```

<span data-ttu-id="99baa-128">Vytvořte hello DaemonSet s hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="99baa-128">Create hello DaemonSet with hello following command:</span></span>

```azurecli-interactive
kubectl create -f oms-daemonset.yaml
```

<span data-ttu-id="99baa-129">toosee této hello DaemonSet je vytvořena, spusťte:</span><span class="sxs-lookup"><span data-stu-id="99baa-129">toosee that hello DaemonSet is created, run:</span></span>

```azurecli-interactive
kubectl get daemonset
```

<span data-ttu-id="99baa-130">Výstup je podobné toohello následující:</span><span class="sxs-lookup"><span data-stu-id="99baa-130">Output is similar toohello following:</span></span>

```azurecli-interactive
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE-SELECTOR   AGE
omsagent   3         3         3         0            3           <none>          5m
```

<span data-ttu-id="99baa-131">Po hello agenty běží, trvá několik minut, než OMS tooingest a zpracování dat hello.</span><span class="sxs-lookup"><span data-stu-id="99baa-131">After hello agents are running, it takes several minutes for OMS tooingest and process hello data.</span></span>

## <a name="access-monitoring-data"></a><span data-ttu-id="99baa-132">Přístup k datům monitorování</span><span class="sxs-lookup"><span data-stu-id="99baa-132">Access monitoring data</span></span>

<span data-ttu-id="99baa-133">Zobrazit a analyzovat monitorování dat pomocí hello kontejneru OMS hello [kontejneru řešení](../../log-analytics/log-analytics-containers.md) v portálu OMS hello nebo hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="99baa-133">View and analyze hello OMS container monitoring data with hello [Container solution](../../log-analytics/log-analytics-containers.md) in either hello OMS portal or hello Azure portal.</span></span> 

<span data-ttu-id="99baa-134">tooinstall hello kontejneru řešení pomocí hello [portálu OMS](https://mms.microsoft.com), přejděte příliš**řešení Galerie**.</span><span class="sxs-lookup"><span data-stu-id="99baa-134">tooinstall hello Container solution using hello [OMS portal](https://mms.microsoft.com), go too**Solutions Gallery**.</span></span> <span data-ttu-id="99baa-135">Pak přidejte **kontejneru řešení**.</span><span class="sxs-lookup"><span data-stu-id="99baa-135">Then add **Container Solution**.</span></span> <span data-ttu-id="99baa-136">Můžete taky přidat hello kontejnery řešení z hello [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft.containersoms?tab=Overview).</span><span class="sxs-lookup"><span data-stu-id="99baa-136">Alternatively, add hello Containers solution from hello [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft.containersoms?tab=Overview).</span></span>

<span data-ttu-id="99baa-137">Na portálu OMS hello, vyhledejte **kontejnery** souhrnné dlaždice na řídicím panelu hello OMS.</span><span class="sxs-lookup"><span data-stu-id="99baa-137">In hello OMS portal, look for a **Containers** summary tile on hello OMS dashboard.</span></span> <span data-ttu-id="99baa-138">Klikněte na dlaždici hello podrobnosti, včetně: události kontejneru, chyb, stav, bitové kopie inventáře a využití procesoru a paměti.</span><span class="sxs-lookup"><span data-stu-id="99baa-138">Click hello tile for details including: container events, errors, status, image inventory, and CPU and memory usage.</span></span> <span data-ttu-id="99baa-139">Podrobnější informace, klikněte na řádek na libovolnou dlaždici nebo provádět [hledání protokolů](../../log-analytics/log-analytics-log-searches.md).</span><span class="sxs-lookup"><span data-stu-id="99baa-139">For more granular information, click a row on any tile, or perform a [log search](../../log-analytics/log-analytics-log-searches.md).</span></span>

![Řídicí panel kontejnery na portálu OMS](./media/container-service-tutorial-kubernetes-monitor/oms-containers-dashboard.png)

<span data-ttu-id="99baa-141">Podobně hello portálu Azure, přejděte v příliš**analýzy protokolů** a vyberte název pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="99baa-141">Similarly, in hello Azure portal, go too**Log Analytics** and select your workspace name.</span></span> <span data-ttu-id="99baa-142">toosee hello **kontejnery** dlaždice souhrnu, klikněte na tlačítko **řešení** > **kontejnery**.</span><span class="sxs-lookup"><span data-stu-id="99baa-142">toosee hello **Containers** summary tile, click **Solutions** > **Containers**.</span></span> <span data-ttu-id="99baa-143">Podrobnosti toosee, klikněte na dlaždici hello.</span><span class="sxs-lookup"><span data-stu-id="99baa-143">toosee details, click hello tile.</span></span>

<span data-ttu-id="99baa-144">V tématu hello [dokumentace Azure Log Analytics](../../log-analytics/index.md) obsahuje podrobné pokyny k dotazování a analýze dat monitorování.</span><span class="sxs-lookup"><span data-stu-id="99baa-144">See hello [Azure Log Analytics documentation](../../log-analytics/index.md) for detailed guidance on querying and analyzing monitoring data.</span></span>

## <a name="next-steps"></a><span data-ttu-id="99baa-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="99baa-145">Next steps</span></span>

<span data-ttu-id="99baa-146">V tomto kurzu sledovat Kubernetes clusteru pomocí OMS.</span><span class="sxs-lookup"><span data-stu-id="99baa-146">In this tutorial, you monitored your Kubernetes cluster with OMS.</span></span> <span data-ttu-id="99baa-147">Úlohy popsané součástí:</span><span class="sxs-lookup"><span data-stu-id="99baa-147">Tasks covered included:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="99baa-148">Získat nastavení pracovní prostor OMS</span><span class="sxs-lookup"><span data-stu-id="99baa-148">Get OMS Workspace settings</span></span>
> * <span data-ttu-id="99baa-149">Nastavit OMS agenty na uzly Kubernetes hello</span><span class="sxs-lookup"><span data-stu-id="99baa-149">Set up OMS agents on hello Kubernetes nodes</span></span>
> * <span data-ttu-id="99baa-150">Přístup k monitorování informací hello OMS portál nebo portál Azure</span><span class="sxs-lookup"><span data-stu-id="99baa-150">Access monitoring information in hello OMS portal or Azure portal</span></span>


<span data-ttu-id="99baa-151">Použijte tento odkaz toosee předem vytvořené skriptu ukázky pro službu kontejneru.</span><span class="sxs-lookup"><span data-stu-id="99baa-151">Follow this link toosee pre-built script samples for Container Service.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="99baa-152">Skript ukázek Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="99baa-152">Azure Container Service script samples</span></span>](cli-samples.md)
