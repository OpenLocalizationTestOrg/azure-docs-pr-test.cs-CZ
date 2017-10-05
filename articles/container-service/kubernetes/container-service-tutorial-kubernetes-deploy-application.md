---
title: "Kurz pro Azure Container Service – nasazení aplikace | Microsoft Docs"
description: "Kurz pro Azure Container Service – nasazení aplikace"
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, Kontejnery, mikroslužby, Kubernetes, DC/OS, Azure"
ms.assetid: 
ms.service: container-service
ms.devlang: aurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: ea67f0beb6a5926393b26e7590302ad0f46a63f9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="run-applications-in-kubernetes"></a>Spuštění aplikace v Kubernetes

V tomto kurzu součástí čtyři 7, vzorová aplikace je nasazený do clusteru s podporou Kubernetes. Dokončit krokům patří:

> [!div class="checklist"]
> * Stáhnout soubory manifestu Kubernetes
> * Spuštění aplikace v Kubernetes
> * Testování aplikace

V následujících kurzech této aplikace je škálovat na více systémů, aktualizovat, a Operations Management Suite konfigurované pro monitorování Kubernetes clusteru.

Tento kurz předpokládá základní znalosti o Kubernetes koncepty, podrobné informace o Kubernetes najdete [Kubernetes dokumentaci](https://kubernetes.io/docs/home/).

## <a name="before-you-begin"></a>Než začnete

V předchozí kurzy aplikace byla zabalené do kontejneru image, tuto bitovou kopii byl odeslán do registru kontejner Azure a Kubernetes cluster byla vytvořena. Pokud se ještě provést tyto kroky a chcete sledovat, vrátit [kurzu 1 – Vytvoření kontejneru image](./container-service-tutorial-kubernetes-prepare-app.md). 

Minimálně tento kurz vyžaduje Kubernetes clusteru.

## <a name="get-manifest-file"></a>Získat soubor manifestu

V tomto kurzu [Kubernetes objekty](https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/) jsou nasazeny pomocí Kubernetes manifestu. Kubernetes manifest je soubor YAML nebo JSON formátovaný obsahující pokyny k nasazení a konfigurace objektu Kubernetes.

Soubor manifestu aplikace pro účely tohoto kurzu je k dispozici v úložišti aplikací Azure hlas, který byl v předchozím kurzu klonovat. Pokud jste tak již neučinili, naklonujte úložiště pomocí následujícího příkazu: 

```bash
git clone https://github.com/Azure-Samples/azure-voting-app-redis.git
```

Soubor manifestu se nachází v následujícím adresáři klonovaný úložišti.

```bash
/azure-voting-app-redis/kubernetes-manifests/azure-vote-all-in-one-redis.yml
```

## <a name="update-manifest-file"></a>Aktualizace souboru manifestu

Pokud používáte Azure kontejneru registru pro ukládání bitových kopií kontejneru, je třeba aktualizovat s názvem loginServer ACR manifest.

Získat název ACR přihlášení serveru s [az acr seznamu](/cli/azure/acr#list) příkaz.

```azurecli-interactive
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Ukázka manifest byl předem vytvořené s názvem úložiště *microsoft*. V každém textovém editoru otevřete soubor a nahraďte *microsoft* hodnotu s názvem serveru přihlášení vaší instance ACR.

```yaml
containers:
- name: azure-vote-front
  image: microsoft/azure-vote-front:redis-v1
```

## <a name="deploy-application"></a>Nasazení aplikace

Pomocí příkazu [kubectl create](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#create) spusťte aplikaci. Tento příkaz analyzuje souboru manifestu a vytvořit objekty definované Kubernetes.

```azurecli-interactive
kubectl create -f ./azure-voting-app-redis/kubernetes-manifests/azure-vote-all-in-one-redis.yml
```

Výstup:

```bash
deployment "azure-vote-back" created
service "azure-vote-back" created
deployment "azure-vote-front" created
service "azure-vote-front" created
```

## <a name="test-application"></a>Testování aplikace

A [Kubernetes služby](https://kubernetes.io/docs/concepts/services-networking/service/) se vytvoří, který zpřístupňuje aplikace k Internetu. Tento proces může trvat několik minut. 

Pomocí příkazu [kubectl get service](https://review.docs.microsoft.com/en-us/azure/container-service/container-service-kubernetes-walkthrough?branch=pr-en-us-17681) s argumentem `--watch` můžete sledovat průběh.

```azurecli-interactive
kubectl get service azure-vote-front --watch
```

Zpočátku **externí IP** pro *azure hlas front* služby se zobrazí jako *čekající*. Jakmile se adresa EXTERNAL-IP změní ze stavu *probíhá* na *IP adresu*, pomocí klávesové zkratky `CTRL-C` zastavte sledovací proces kubectl.

```bash
NAME               CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   10.0.42.158   <pending>     80:31873/TCP   1m
azure-vote-front   10.0.42.158   52.179.23.131 80:31873/TCP   2m
```

Informace o aplikaci, přejděte na externí IP adresu.

![Obrázek clusteru Kubernetes v Azure](media/container-service-kubernetes-tutorials/azure-vote.png)

## <a name="next-steps"></a>Další kroky

V tomto kurzu aplikace Azure hlas nasazená do clusteru Azure Container Service Kubernetes. Dokončené úkoly patří:  

> [!div class="checklist"]
> * Stáhnout soubory manifestu Kubernetes
> * Spusťte aplikaci v Kubernetes
> * Testování aplikace

Přechodu na v dalším kurzu se dozvíte o škálování Kubernetes aplikace a podpůrné infrastruktuře Kubernetes. 

> [!div class="nextstepaction"]
> [Škálování Kubernetes aplikace a infrastrukturu](./container-service-tutorial-kubernetes-scale.md)