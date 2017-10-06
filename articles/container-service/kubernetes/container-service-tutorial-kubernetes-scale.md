---
title: "kurz pro službu kontejneru aaaAzure – škálování aplikace | Microsoft Docs"
description: "Kurz pro Azure Container Service - škálování aplikace"
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
ms.openlocfilehash: 29571eef0fd91bd6b40f00d8c018539f320179bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="scale-kubernetes-pods-and-kubernetes-infrastructure"></a>Škálování infrastruktury Kubernetes a pracovními stanicemi soustředěnými kolem Kubernetes

Pokud jste byli po hello kurzy, máte funkční Kubernetes cluster v Azure Container Service a nasazení aplikace Azure hlasování hello. 

V tomto kurzu, součástí pět 7, škálovat hello pracovními stanicemi soustředěnými kolem v hello aplikaci a akci pod automatické škálování. Také zjistíte, jak tooscale hello počet toochange uzly agenta virtuálního počítače Azure hello kapacity na clusteru pro hostování úloh. Dokončené úkoly patří:

> [!div class="checklist"]
> * Ruční škálování pracovními stanicemi soustředěnými kolem Kubernetes
> * Konfigurace automatického škálování pracovními stanicemi soustředěnými kolem systémem hello aplikace front-endu
> * Škálování hello Kubernetes Azure agent uzly

V následujících kurzech se aktualizuje hello hlas Azure aplikace a služby Operations Management Suite nakonfigurovaný toomonitor hello Kubernetes clusteru.

## <a name="before-you-begin"></a>Než začnete

V předchozí kurzy aplikace se zabalí do kontejneru image, tuto bitovou kopii nahráli tooAzure registru kontejneru a cluster Kubernetes vytvořit. Hello aplikace pak byl na hello Kubernetes clusteru spusťte. Pokud jste ještě provést tyto kroky a chcete toofollow společně, vrátí toohello [kurzu 1 – Vytvoření kontejneru image](./container-service-tutorial-kubernetes-prepare-app.md). 

Minimálně tento kurz vyžaduje Kubernetes clusteru s běžící aplikaci.

## <a name="manually-scale-pods"></a>Ručně škálovat, pracovními stanicemi soustředěnými kolem

Proto daleko hello Azure hlas front-endu a Redis instance nainstalovaná, každý s jednu repliku. tooverify, spusťte hello [kubectl získat](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) příkaz.

```azurecli-interactive
kubectl get pods
```

Výstup:

```bash
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-2549686872-4d2r5   1/1       Running   0          31m
azure-vote-front-848767080-tf34m   1/1       Running   0          31m
```

Ruční změna hello počet pracovními stanicemi soustředěnými kolem v hello `azure-vote-front` nasazení pomocí hello [kubectl škálování](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#scale) příkaz. Tento příklad zvyšuje počet too5 hello.

```azurecli-interactive
kubectl scale --replicas=5 deployment/azure-vote-front
```

Spustit [kubectl získat pracovními stanicemi soustředěnými kolem](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) tooverify, Kubernetes vytváří pracovními stanicemi soustředěnými kolem hello. Za minutu jsou spuštěny další pracovními stanicemi soustředěnými kolem hello:

```azurecli-interactive
kubectl get pods
```

Výstup:

```bash
NAME                                READY     STATUS    RESTARTS   AGE
azure-vote-back-2606967446-nmpcf    1/1       Running   0          15m
azure-vote-front-3309479140-2hfh0   1/1       Running   0          3m
azure-vote-front-3309479140-bzt05   1/1       Running   0          3m
azure-vote-front-3309479140-fvcvm   1/1       Running   0          3m
azure-vote-front-3309479140-hrbf2   1/1       Running   0          15m
azure-vote-front-3309479140-qphz8   1/1       Running   0          3m
```

## <a name="autoscale-pods"></a>Pracovními stanicemi soustředěnými kolem škálování

Podporuje Kubernetes [automatické škálování vodorovné pod](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) tooadjust hello počet pracovními stanicemi soustředěnými kolem v nasazení v závislosti na využití procesoru nebo jiné vybrat metriky. 

toouse hello autoscaler, vaše pracovními stanicemi soustředěnými kolem musí mít procesor požadavky a omezení definovaná. V hello `azure-vote-front` nasazení hello front-end kontejneru požadavky 0,25 procesor limit 0,5 procesoru. nastavení Hello vypadat podobně jako:

```YAML
resources:
  requests:
     cpu: 250m
  limits:
     cpu: 500m
```

Hello následující příklad používá hello [kubectl škálování](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#autoscale) příkaz tooautoscale hello počet pracovními stanicemi soustředěnými kolem v hello `azure-vote-front` nasazení. Sem pokud využití procesoru přesahuje 50 %, hello autoscaler zvyšuje hello pracovními stanicemi soustředěnými kolem tooa maximálně 10.


```azurecli-interactive
kubectl autoscale deployment azure-vote-front --cpu-percent=50 --min=3 --max=10
```

Stav hello toosee hello autoscaler, spusťte následující příkaz hello:

```azurecli-interactive
kubectl get hpa
```

Výstup:

```bash
NAME               REFERENCE                     TARGETS    MINPODS   MAXPODS   REPLICAS   AGE
azure-vote-front   Deployment/azure-vote-front   0% / 50%   3         10        3          2m
```

Po několika minutách, s minimálním zatížením hello aplikace Azure hlas snižuje hello počet replik pod automaticky too3.

## <a name="scale-hello-agents"></a>Škálování hello agentů

Pokud jste vytvořili pomocí výchozí příkazů v předchozí kurzu hello Kubernetes clusteru, má tři uzly agenta. Hello počtu agentů, můžete upravit ručně, pokud máte v plánu více nebo méně kontejneru zatížení v clusteru. Použití hello [škálování služby acs az](/cli/azure/acs#scale) příkaz, a zadejte číslo hello agentů s hello `--new-agent-count` parametr.

Hello následující příklad zvyšuje hello počet agenta too4 uzly v clusteru Kubernetes hello s názvem *myK8sCluster*. příkaz Hello bude trvat několik minut toocomplete.

```azurecli-interactive
az acs scale --resource-group=myResourceGroup --name=myK8SCluster --new-agent-count 4
```

Hello výstupu příkazu se zobrazí hello číslo agenta uzly v hello hodnotu `agentPoolProfiles:count`:

```azurecli
{
  "agentPoolProfiles": [
    {
      "count": 4,
      "dnsPrefix": "myK8SCluster-myK8SCluster-e44f25-k8s-agents",
      "fqdn": "",
      "name": "agentpools",
      "vmSize": "Standard_D2_v2"
    }
  ],
...

```

## <a name="next-steps"></a>Další kroky

V tomto kurzu použít v clusteru Kubernetes různé funkce škálování. Úlohy popsané součástí:

> [!div class="checklist"]
> * Ruční škálování pracovními stanicemi soustředěnými kolem Kubernetes
> * Konfigurace automatického škálování pracovními stanicemi soustředěnými kolem systémem hello aplikace front-endu
> * Škálování hello Kubernetes Azure agent uzly

Další kurz toolearn toohello o aktualizaci aplikace v Kubernetes zálohy.

> [!div class="nextstepaction"]
> [Aktualizace aplikace v Kubernetes](./container-service-tutorial-kubernetes-app-update.md)

