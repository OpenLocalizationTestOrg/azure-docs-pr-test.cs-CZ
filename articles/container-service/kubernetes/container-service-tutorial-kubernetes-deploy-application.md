---
title: "kurz pro službu kontejneru aaaAzure – nasazení aplikace | Microsoft Docs"
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
ms.openlocfilehash: 7e2fa06d359caf83e684df3966624a6e9a8e7efa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-applications-in-kubernetes"></a>Spuštění aplikace v Kubernetes

V tomto kurzu součástí čtyři 7, vzorová aplikace je nasazený do clusteru s podporou Kubernetes. Dokončit krokům patří:

> [!div class="checklist"]
> * Stáhnout soubory manifestu Kubernetes
> * Spuštění aplikace v Kubernetes
> * Testování aplikace hello

V následujících kurzech této aplikace je škálovat na více systémů, aktualizovat, a nakonfigurovat Operations Management Suite toomonitor hello Kubernetes clusteru.

Tento kurz předpokládá základní znalosti koncepce Kubernetes najdete podrobné informace o Kubernetes hello [Kubernetes dokumentaci](https://kubernetes.io/docs/home/).

## <a name="before-you-begin"></a>Než začnete

V předchozí kurzy aplikace se zabalí do kontejneru image, tuto bitovou kopii, byla nahrané tooAzure registru kontejneru a byl vytvořen Kubernetes cluster. Pokud jste ještě provést tyto kroky a chcete toofollow společně, vrátí příliš[kurzu 1 – Vytvoření kontejneru image](./container-service-tutorial-kubernetes-prepare-app.md). 

Minimálně tento kurz vyžaduje Kubernetes clusteru.

## <a name="get-manifest-file"></a>Získat soubor manifestu

V tomto kurzu [Kubernetes objekty](https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/) jsou nasazeny pomocí Kubernetes manifestu. Kubernetes manifest je soubor YAML nebo JSON formátovaný obsahující pokyny k nasazení a konfigurace objektu Kubernetes.

Hello souboru manifestu aplikace pro účely tohoto kurzu je k dispozici v úložišti aplikací Azure hlas hello, který byl v předchozím kurzu klonovat. Pokud jste tak již neučinili, klonovat úložiště hello s hello následující příkaz: 

```bash
git clone https://github.com/Azure-Samples/azure-voting-app-redis.git
```

Soubor manifestu Hello nachází v následujícím adresáři hello klonovat úložiště hello.

```bash
/azure-voting-app-redis/kubernetes-manifests/azure-vote-all-in-one-redis.yml
```

## <a name="update-manifest-file"></a>Aktualizace souboru manifestu

Pokud používáte Azure kontejneru registru toostore hello kontejneru obrázky, hello manifestu potřebám toobe aktualizované hello loginServer název ACR.

Získat název serveru aplikace hello ACR přihlášení s hello [az acr seznamu](/cli/azure/acr#list) příkaz.

```azurecli-interactive
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Hello manifest ukázka byla předem vytvořené s názvem úložiště *microsoft*. Otevřete soubor hello v každém textovém editoru a nahraďte hello *microsoft* hodnotu s názvem serveru hello přihlášení vaší instance ACR.

```yaml
containers:
- name: azure-vote-front
  image: microsoft/azure-vote-front:redis-v1
```

## <a name="deploy-application"></a>Nasazení aplikace

Použití hello [kubectl vytvořit](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#create) příkaz toorun hello aplikace. Tento příkaz analyzuje hello soubor manifestu a vytvořit objekty Kubernetes hello definované.

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

A [Kubernetes služby](https://kubernetes.io/docs/concepts/services-networking/service/) se vytvoří, který zpřístupňuje toohello aplikace hello Internetu. Tento proces může trvat několik minut. 

toomonitor průběh, použijte hello [kubectl získat služby](https://review.docs.microsoft.com/en-us/azure/container-service/container-service-kubernetes-walkthrough?branch=pr-en-us-17681) s hello `--watch` argument.

```azurecli-interactive
kubectl get service azure-vote-front --watch
```

Na začátku hello **externí IP** pro hello *azure hlas front* služby se zobrazí jako *čekající*. Jakmile hello externí IP adresu se změnil z *čekající* tooan *IP adresu*, použijte `CTRL-C` toostop hello kubectl sledovat proces.

```bash
NAME               CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   10.0.42.158   <pending>     80:31873/TCP   1m
azure-vote-front   10.0.42.158   52.179.23.131 80:31873/TCP   2m
```

toosee hello aplikaci, procházejte toohello externí IP adresu.

![Obrázek clusteru Kubernetes v Azure](media/container-service-kubernetes-tutorials/azure-vote.png)

## <a name="next-steps"></a>Další kroky

V tomto kurzu se hello Azure hlas aplikace nasazené tooan clusteru Azure Container Service Kubernetes. Dokončené úkoly patří:  

> [!div class="checklist"]
> * Stáhnout soubory manifestu Kubernetes
> * Spuštění aplikace hello v Kubernetes
> * Aplikace otestované hello

Posunutí další kurz toolearn toohello o škálování aplikace Kubernetes i hello základní Kubernetes infrastruktury. 

> [!div class="nextstepaction"]
> [Škálování Kubernetes aplikace a infrastrukturu](./container-service-tutorial-kubernetes-scale.md)