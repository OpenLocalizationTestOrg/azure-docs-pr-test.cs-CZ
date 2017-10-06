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
# <a name="monitor-a-kubernetes-cluster-with-operations-management-suite"></a>Monitorování Kubernetes clusteru se službou Operations Management Suite

Monitorování Kubernetes clusteru a kontejnerů, je důležité, zejména v případě, že spravujete provozní cluster ve velkém měřítku s více aplikacemi. 

Můžete využít výhod několik Kubernetes řešení monitorování, buď od společnosti Microsoft nebo jiní poskytovatelé. V tomto kurzu můžete monitorovat Kubernetes clusteru pomocí řešení hello kontejnery v [Operations Management Suite](../../operations-management-suite/operations-management-suite-overview.md), řešení správy založené na cloudu IT společnosti Microsoft. (hello OMS kontejnery řešení je ve verzi preview.)

V tomto kurzu součástí sedm 7, obsahuje hello následující úlohy:

> [!div class="checklist"]
> * Získat nastavení pracovní prostor OMS
> * Nastavit OMS agenty na uzly Kubernetes hello
> * Přístup k monitorování informací hello OMS portál nebo portál Azure

## <a name="before-you-begin"></a>Než začnete

V předchozích kurzy aplikace se zabalí do kontejneru bitové kopie, těchto bitových kopií nahráli tooAzure registru kontejneru a cluster Kubernetes vytvořit. Pokud jste ještě provést tyto kroky a chcete toofollow společně, vrátí příliš[kurzu 1 – Vytvoření kontejneru image](./container-service-tutorial-kubernetes-prepare-app.md). 

Minimálně tento kurz vyžaduje Kubernetes cluster s uzly agenta systému Linux a účet služby Operations Management Suite (OMS). V případě potřeby si zaregistrovat [bezplatnou zkušební verzi OMS](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial).

## <a name="get-workspace-settings"></a>Získat nastavení pracovního prostoru

Když dostanete hello [portálu OMS](https://mms.microsoft.com), přejděte příliš**nastavení** > **připojené zdroje** > **servery se systémem Linux**. Zde můžete najít hello *ID pracovního prostoru* a primární nebo sekundární *klíč pracovního prostoru*. Poznamenejte si tyto hodnoty, které budete potřebovat tooset až OMS agenty na hello clusteru.

## <a name="set-up-oms-agents"></a>Nastavit OMS agentů

Zde je souboru tooset YAML až OMS agenty na uzly clusteru Linux hello. Vytvoří Kubernetes [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/), která se spouští jednu identické pod na každém uzlu clusteru. Hello DaemonSet prostředků je ideální pro nasazení agenta monitorování. 

Uložit hello následující text tooa soubor s názvem `oms-daemonset.yaml`a nahraďte zástupný symbol hodnoty hello *myWorkspaceID* a *myWorkspaceKey* s ID pracovního prostoru OMS a klíč. (V produkčním prostředí, můžete zakódovat tyto hodnoty jako tajných klíčů.)

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

Vytvořte hello DaemonSet s hello následující příkaz:

```azurecli-interactive
kubectl create -f oms-daemonset.yaml
```

toosee této hello DaemonSet je vytvořena, spusťte:

```azurecli-interactive
kubectl get daemonset
```

Výstup je podobné toohello následující:

```azurecli-interactive
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE-SELECTOR   AGE
omsagent   3         3         3         0            3           <none>          5m
```

Po hello agenty běží, trvá několik minut, než OMS tooingest a zpracování dat hello.

## <a name="access-monitoring-data"></a>Přístup k datům monitorování

Zobrazit a analyzovat monitorování dat pomocí hello kontejneru OMS hello [kontejneru řešení](../../log-analytics/log-analytics-containers.md) v portálu OMS hello nebo hello portálu Azure. 

tooinstall hello kontejneru řešení pomocí hello [portálu OMS](https://mms.microsoft.com), přejděte příliš**řešení Galerie**. Pak přidejte **kontejneru řešení**. Můžete taky přidat hello kontejnery řešení z hello [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft.containersoms?tab=Overview).

Na portálu OMS hello, vyhledejte **kontejnery** souhrnné dlaždice na řídicím panelu hello OMS. Klikněte na dlaždici hello podrobnosti, včetně: události kontejneru, chyb, stav, bitové kopie inventáře a využití procesoru a paměti. Podrobnější informace, klikněte na řádek na libovolnou dlaždici nebo provádět [hledání protokolů](../../log-analytics/log-analytics-log-searches.md).

![Řídicí panel kontejnery na portálu OMS](./media/container-service-tutorial-kubernetes-monitor/oms-containers-dashboard.png)

Podobně hello portálu Azure, přejděte v příliš**analýzy protokolů** a vyberte název pracovního prostoru. toosee hello **kontejnery** dlaždice souhrnu, klikněte na tlačítko **řešení** > **kontejnery**. Podrobnosti toosee, klikněte na dlaždici hello.

V tématu hello [dokumentace Azure Log Analytics](../../log-analytics/index.md) obsahuje podrobné pokyny k dotazování a analýze dat monitorování.

## <a name="next-steps"></a>Další kroky

V tomto kurzu sledovat Kubernetes clusteru pomocí OMS. Úlohy popsané součástí:

> [!div class="checklist"]
> * Získat nastavení pracovní prostor OMS
> * Nastavit OMS agenty na uzly Kubernetes hello
> * Přístup k monitorování informací hello OMS portál nebo portál Azure


Použijte tento odkaz toosee předem vytvořené skriptu ukázky pro službu kontejneru.

> [!div class="nextstepaction"]
> [Skript ukázek Azure Container Service](cli-samples.md)
