---
title: "Kurz pro Azure Container Service - Kubernetes monitorování | Microsoft Docs"
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
ms.openlocfilehash: f4d09973ada8e3cd0ff2b00d20aca979e834cd7f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-a-kubernetes-cluster-with-operations-management-suite"></a><span data-ttu-id="ff6f5-104">Monitorování Kubernetes clusteru se službou Operations Management Suite</span><span class="sxs-lookup"><span data-stu-id="ff6f5-104">Monitor a Kubernetes cluster with Operations Management Suite</span></span>

<span data-ttu-id="ff6f5-105">Monitorování Kubernetes clusteru a kontejnerů, je důležité, zejména v případě, že spravujete provozní cluster ve velkém měřítku s více aplikacemi.</span><span class="sxs-lookup"><span data-stu-id="ff6f5-105">Monitoring your Kubernetes cluster and containers is critical, especially when you manage a production cluster at scale with multiple apps.</span></span> 

<span data-ttu-id="ff6f5-106">Můžete využít výhod několik Kubernetes řešení monitorování, buď od společnosti Microsoft nebo jiní poskytovatelé.</span><span class="sxs-lookup"><span data-stu-id="ff6f5-106">You can take advantage of several Kubernetes monitoring solutions, either from Microsoft or other providers.</span></span> <span data-ttu-id="ff6f5-107">V tomto kurzu můžete Kubernetes cluster monitorovat pomocí řešení kontejnery v [Operations Management Suite](../../operations-management-suite/operations-management-suite-overview.md), řešení správy založené na cloudu IT společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="ff6f5-107">In this tutorial, you monitor your Kubernetes cluster by using the Containers solution in [Operations Management Suite](../../operations-management-suite/operations-management-suite-overview.md), Microsoft's cloud-based IT management solution.</span></span> <span data-ttu-id="ff6f5-108">(OMS kontejnery řešení je ve verzi preview.)</span><span class="sxs-lookup"><span data-stu-id="ff6f5-108">(The OMS Containers solution is in preview.)</span></span>

<span data-ttu-id="ff6f5-109">V tomto kurzu součástí sedm 7, obsahuje následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="ff6f5-109">This tutorial, part seven of seven, covers the following tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ff6f5-110">Získat nastavení pracovní prostor OMS</span><span class="sxs-lookup"><span data-stu-id="ff6f5-110">Get OMS Workspace settings</span></span>
> * <span data-ttu-id="ff6f5-111">Nastavit OMS agentů na uzlech Kubernetes</span><span class="sxs-lookup"><span data-stu-id="ff6f5-111">Set up OMS agents on the Kubernetes nodes</span></span>
> * <span data-ttu-id="ff6f5-112">Přístup k monitorování informací portálu OMS nebo portálu Azure</span><span class="sxs-lookup"><span data-stu-id="ff6f5-112">Access monitoring information in the OMS portal or Azure portal</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="ff6f5-113">Než začnete</span><span class="sxs-lookup"><span data-stu-id="ff6f5-113">Before you begin</span></span>

<span data-ttu-id="ff6f5-114">V předchozích kurzech byla aplikace zabalené do kontejneru obrázků, tyto Image nahrané do registru kontejner Azure a cluster Kubernetes vytvořili.</span><span class="sxs-lookup"><span data-stu-id="ff6f5-114">In previous tutorials, an application was packaged into container images, these images uploaded to Azure Container Registry, and a Kubernetes cluster created.</span></span> <span data-ttu-id="ff6f5-115">Pokud se ještě provést tyto kroky a chcete sledovat, vrátit [kurzu 1 – Vytvoření kontejneru image](./container-service-tutorial-kubernetes-prepare-app.md).</span><span class="sxs-lookup"><span data-stu-id="ff6f5-115">If you have not done these steps, and would like to follow along, return to [Tutorial 1 – Create container images](./container-service-tutorial-kubernetes-prepare-app.md).</span></span> 

<span data-ttu-id="ff6f5-116">Minimálně tento kurz vyžaduje Kubernetes cluster s uzly agenta systému Linux a účet služby Operations Management Suite (OMS).</span><span class="sxs-lookup"><span data-stu-id="ff6f5-116">At minimum, this tutorial requires a Kubernetes cluster with Linux agent nodes, and an Operations Management Suite (OMS) account.</span></span> <span data-ttu-id="ff6f5-117">V případě potřeby si zaregistrovat [bezplatnou zkušební verzi OMS](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial).</span><span class="sxs-lookup"><span data-stu-id="ff6f5-117">If needed, sign up for a [free OMS trial](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial).</span></span>

## <a name="get-workspace-settings"></a><span data-ttu-id="ff6f5-118">Získat nastavení pracovního prostoru</span><span class="sxs-lookup"><span data-stu-id="ff6f5-118">Get Workspace settings</span></span>

<span data-ttu-id="ff6f5-119">Když se dostanete [portálu OMS](https://mms.microsoft.com), přejděte na **nastavení** > **připojené zdroje** > **servery se systémem Linux**.</span><span class="sxs-lookup"><span data-stu-id="ff6f5-119">When you can access the [OMS portal](https://mms.microsoft.com), go to **Settings** > **Connected Sources** > **Linux Servers**.</span></span> <span data-ttu-id="ff6f5-120">Zde můžete najít *ID pracovního prostoru* a primární nebo sekundární *klíč pracovního prostoru*.</span><span class="sxs-lookup"><span data-stu-id="ff6f5-120">There, you can find the *Workspace ID* and a primary or secondary *Workspace Key*.</span></span> <span data-ttu-id="ff6f5-121">Poznamenejte si tyto hodnoty, které budete muset nastavit OMS agentů v clusteru.</span><span class="sxs-lookup"><span data-stu-id="ff6f5-121">Take note of these values, which you need to set up OMS agents on the cluster.</span></span>

## <a name="set-up-oms-agents"></a><span data-ttu-id="ff6f5-122">Nastavit OMS agentů</span><span class="sxs-lookup"><span data-stu-id="ff6f5-122">Set up OMS agents</span></span>

<span data-ttu-id="ff6f5-123">Zde je soubor YAML nastavit OMS agenty na uzly clusteru Linux.</span><span class="sxs-lookup"><span data-stu-id="ff6f5-123">Here is a YAML file to set up OMS agents on the Linux cluster nodes.</span></span> <span data-ttu-id="ff6f5-124">Vytvoří Kubernetes [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/), která se spouští jednu identické pod na každém uzlu clusteru.</span><span class="sxs-lookup"><span data-stu-id="ff6f5-124">It creates a Kubernetes [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/), which runs a single identical pod on each cluster node.</span></span> <span data-ttu-id="ff6f5-125">Prostředek DaemonSet je ideální pro nasazení agenta monitorování.</span><span class="sxs-lookup"><span data-stu-id="ff6f5-125">The DaemonSet resource is ideal for deploying a monitoring agent.</span></span> 

<span data-ttu-id="ff6f5-126">Uložte následující text do souboru s názvem `oms-daemonset.yaml`a nahraďte zástupný symbol hodnoty pro *myWorkspaceID* a *myWorkspaceKey* s ID pracovního prostoru OMS a klíč.</span><span class="sxs-lookup"><span data-stu-id="ff6f5-126">Save the following text to a file named `oms-daemonset.yaml`, and replace the placeholder values for *myWorkspaceID* and *myWorkspaceKey* with your OMS Workspace ID and Key.</span></span> <span data-ttu-id="ff6f5-127">(V produkčním prostředí, můžete zakódovat tyto hodnoty jako tajných klíčů.)</span><span class="sxs-lookup"><span data-stu-id="ff6f5-127">(In production, you can encode these values as secrets.)</span></span>

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

<span data-ttu-id="ff6f5-128">Vytvořte DaemonSet pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="ff6f5-128">Create the DaemonSet with the following command:</span></span>

```azurecli-interactive
kubectl create -f oms-daemonset.yaml
```

<span data-ttu-id="ff6f5-129">Pokud chcete zjistit, jestli je vytvořená DaemonSet, spusťte příkaz:</span><span class="sxs-lookup"><span data-stu-id="ff6f5-129">To see that the DaemonSet is created, run:</span></span>

```azurecli-interactive
kubectl get daemonset
```

<span data-ttu-id="ff6f5-130">Výstup je podobný tomuto:</span><span class="sxs-lookup"><span data-stu-id="ff6f5-130">Output is similar to the following:</span></span>

```azurecli-interactive
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE-SELECTOR   AGE
omsagent   3         3         3         0            3           <none>          5m
```

<span data-ttu-id="ff6f5-131">Po agenty běží, trvá několik minut, než OMS ingestování a zpracovat data.</span><span class="sxs-lookup"><span data-stu-id="ff6f5-131">After the agents are running, it takes several minutes for OMS to ingest and process the data.</span></span>

## <a name="access-monitoring-data"></a><span data-ttu-id="ff6f5-132">Přístup k datům monitorování</span><span class="sxs-lookup"><span data-stu-id="ff6f5-132">Access monitoring data</span></span>

<span data-ttu-id="ff6f5-133">Zobrazit a analyzovat monitorování dat pomocí kontejneru OMS [kontejneru řešení](../../log-analytics/log-analytics-containers.md) v portálu OMS nebo portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ff6f5-133">View and analyze the OMS container monitoring data with the [Container solution](../../log-analytics/log-analytics-containers.md) in either the OMS portal or the Azure portal.</span></span> 

<span data-ttu-id="ff6f5-134">K instalaci kontejneru řešení pomocí [portálu OMS](https://mms.microsoft.com), přejděte na **řešení Galerie**.</span><span class="sxs-lookup"><span data-stu-id="ff6f5-134">To install the Container solution using the [OMS portal](https://mms.microsoft.com), go to **Solutions Gallery**.</span></span> <span data-ttu-id="ff6f5-135">Pak přidejte **kontejneru řešení**.</span><span class="sxs-lookup"><span data-stu-id="ff6f5-135">Then add **Container Solution**.</span></span> <span data-ttu-id="ff6f5-136">Můžete taky přidat kontejnery řešení z [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft.containersoms?tab=Overview).</span><span class="sxs-lookup"><span data-stu-id="ff6f5-136">Alternatively, add the Containers solution from the [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft.containersoms?tab=Overview).</span></span>

<span data-ttu-id="ff6f5-137">Na portálu OMS, vyhledejte **kontejnery** souhrnné dlaždice na řídicím panelu OMS.</span><span class="sxs-lookup"><span data-stu-id="ff6f5-137">In the OMS portal, look for a **Containers** summary tile on the OMS dashboard.</span></span> <span data-ttu-id="ff6f5-138">Klikněte na dlaždici podrobnosti, včetně: události kontejneru, chyb, stav, bitové kopie inventáře a využití procesoru a paměti.</span><span class="sxs-lookup"><span data-stu-id="ff6f5-138">Click the tile for details including: container events, errors, status, image inventory, and CPU and memory usage.</span></span> <span data-ttu-id="ff6f5-139">Podrobnější informace, klikněte na řádek na libovolnou dlaždici nebo provádět [hledání protokolů](../../log-analytics/log-analytics-log-searches.md).</span><span class="sxs-lookup"><span data-stu-id="ff6f5-139">For more granular information, click a row on any tile, or perform a [log search](../../log-analytics/log-analytics-log-searches.md).</span></span>

![Řídicí panel kontejnery na portálu OMS](./media/container-service-tutorial-kubernetes-monitor/oms-containers-dashboard.png)

<span data-ttu-id="ff6f5-141">Podobně v portálu Azure přejděte do **analýzy protokolů** a vyberte název pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="ff6f5-141">Similarly, in the Azure portal, go to **Log Analytics** and select your workspace name.</span></span> <span data-ttu-id="ff6f5-142">Chcete-li zobrazit **kontejnery** dlaždice souhrnu, klikněte na tlačítko **řešení** > **kontejnery**.</span><span class="sxs-lookup"><span data-stu-id="ff6f5-142">To see the **Containers** summary tile, click **Solutions** > **Containers**.</span></span> <span data-ttu-id="ff6f5-143">Chcete-li zobrazit podrobnosti, klikněte na dlaždici.</span><span class="sxs-lookup"><span data-stu-id="ff6f5-143">To see details, click the tile.</span></span>

<span data-ttu-id="ff6f5-144">Najdete v článku [dokumentace Azure Log Analytics](../../log-analytics/index.md) obsahuje podrobné pokyny k dotazování a analýze dat monitorování.</span><span class="sxs-lookup"><span data-stu-id="ff6f5-144">See the [Azure Log Analytics documentation](../../log-analytics/index.md) for detailed guidance on querying and analyzing monitoring data.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ff6f5-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ff6f5-145">Next steps</span></span>

<span data-ttu-id="ff6f5-146">V tomto kurzu sledovat Kubernetes clusteru pomocí OMS.</span><span class="sxs-lookup"><span data-stu-id="ff6f5-146">In this tutorial, you monitored your Kubernetes cluster with OMS.</span></span> <span data-ttu-id="ff6f5-147">Úlohy popsané součástí:</span><span class="sxs-lookup"><span data-stu-id="ff6f5-147">Tasks covered included:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ff6f5-148">Získat nastavení pracovní prostor OMS</span><span class="sxs-lookup"><span data-stu-id="ff6f5-148">Get OMS Workspace settings</span></span>
> * <span data-ttu-id="ff6f5-149">Nastavit OMS agentů na uzlech Kubernetes</span><span class="sxs-lookup"><span data-stu-id="ff6f5-149">Set up OMS agents on the Kubernetes nodes</span></span>
> * <span data-ttu-id="ff6f5-150">Přístup k monitorování informací portálu OMS nebo portálu Azure</span><span class="sxs-lookup"><span data-stu-id="ff6f5-150">Access monitoring information in the OMS portal or Azure portal</span></span>


<span data-ttu-id="ff6f5-151">Tento odkaz zobrazíte předdefinovaných skriptu ukázky pro službu kontejneru.</span><span class="sxs-lookup"><span data-stu-id="ff6f5-151">Follow this link to see pre-built script samples for Container Service.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="ff6f5-152">Skript ukázek Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="ff6f5-152">Azure Container Service script samples</span></span>](cli-samples.md)
