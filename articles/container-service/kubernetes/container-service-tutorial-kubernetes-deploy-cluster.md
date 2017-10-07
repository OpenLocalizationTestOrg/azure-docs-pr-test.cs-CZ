---
title: "kurz pro službu kontejneru aaaAzure – nasazení clusteru | Microsoft Docs"
description: "Kurz pro Azure Container Service – nasazení clusteru"
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, Kontejnery, mikroslužby, Kubernetes, DC/OS, Azure"
ms.assetid: 
ms.service: container-service
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/21/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: c4c8cc95c88d9c2077d0322f57e5d3159e2dd0ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-kubernetes-cluster-in-azure-container-service"></a>Nasazení clusteru Kubernetes v Azure Container Service

Kubernetes poskytuje distribuovanou platformu pro kontejnerizované aplikace. Zřizování clusteru výroby připravené Kubernetes s Azure Container Service je usnadňují a urychlují. V tomto kurzu, část 3 7, je nasazení clusteru Azure Container Service Kubernetes služby. Dokončit krokům patří:

> [!div class="checklist"]
> * Nasazení služby ACS Kubernetes clusteru
> * Instalace hello Kubernetes rozhraní příkazového řádku (kubectl)
> * Konfigurace kubectl

V následujících kurzech hello hlas Azure je aplikace nasazena toohello clusteru, škálovat, aktualizovat a Operations Management Suite je nakonfigurované toomonitor hello Kubernetes clusteru.

## <a name="before-you-begin"></a>Než začnete

V předchozích kurzy bitovou kopii kontejner byl vytvořen a nahrát tooan registru kontejner Azure instance. Pokud jste ještě provést tyto kroky a chcete toofollow společně, vrátí příliš[kurzu 1 – Vytvoření kontejneru image](./container-service-tutorial-kubernetes-prepare-app.md).

## <a name="create-kubernetes-cluster"></a>Vytvoření clusteru Kubernetes

V hello [předchozí kurzu](./container-service-tutorial-kubernetes-prepare-acr.md), skupinu prostředků s názvem *myResourceGroup* byla vytvořena. Pokud jste tak dosud neučinili, vytvořte tato skupina prostředků teď.

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```

Vytvoření clusteru Kubernetes v Azure Container Service s hello [vytvořit acs az](/cli/azure/acs#create) příkaz. 

Hello následující příklad vytvoří cluster s názvem *myK8sCluster* s Linuxem jeden hlavní uzel a tři uzly Linux agent.

```azurecli-interactive 
az acs create --orchestrator-type=kubernetes --resource-group myResourceGroup --name=myK8SCluster --generate-ssh-keys 
```

Po několika minutách dokončení příkazu hello a formátu json vrátí informace o nasazení hello služby ACS.

## <a name="install-hello-kubectl-cli"></a>Nainstalujte hello kubectl rozhraní příkazového řádku

tooconnect toohello Kubernetes clusteru z klientského počítače, použijte [kubectl](https://kubernetes.io/docs/user-guide/kubectl/), hello Kubernetes v klientu příkazového řádku. 

Pokud používáte Azure CloudShell, `kubectl` je už nainstalován. Pokud chcete, aby tooinstall ho místně, použijte hello [az acs kubernetes instalace rozhraní příkazového řádku](/cli/azure/acs/kubernetes#install-cli) příkaz.

Pokud provozujete v systému Linux nebo systému macOS, může být nutné toorun pomocí příkazu "sudo". V systému Windows Ujistěte se, že vaše prostředí byl spuštěn jako správce.

```azurecli-interactive 
az acs kubernetes install-cli 
```

V systému Windows, je výchozí instalace hello *c:\program files (x86)\kubectl.exe*. Může být nutné tooadd tuto cestu k souboru toohello systému Windows. 

## <a name="connect-with-kubectl"></a>Připojení přes kubectl

tooconfigure `kubectl` tooconnect tooyour Kubernetes clusteru, spusťte hello [az acs kubernetes get pověření](/cli/azure/acs/kubernetes#get-credentials) příkaz.

```azurecli-interactive 
az acs kubernetes get-credentials --resource-group=myResourceGroup --name=myK8SCluster
```

tooverify hello připojení tooyour clusteru, spusťte hello [kubectl získat uzly](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) příkaz.

```azurecli-interactive
kubectl get nodes
```

Výstup:

```bash
NAME                    STATUS                     AGE       VERSION
k8s-agent-98dc3136-0    Ready                      5m        v1.6.2
k8s-agent-98dc3136-1    Ready                      5m        v1.6.2
k8s-agent-98dc3136-2    Ready                      5m        v1.6.2
k8s-master-98dc3136-0   Ready,SchedulingDisabled   5m        v1.6.2
```

V kurzu dokončení máte připravené pro zatížení clusteru služby ACS Kubernetes. V následujících kurzech je aplikace s více kontejnerů nasazené toothis cluster, škálovat na více systémů, aktualizovat a sledovat.

## <a name="next-steps"></a>Další kroky

V tomto kurzu byl nasazen clusteru Azure Container Service Kubernetes. byly dokončeny Hello následující kroky:

> [!div class="checklist"]
> * Nasazení clusteru s podporou Kubernetes ACS
> * Nainstalované hello Kubernetes rozhraní příkazového řádku (kubectl)
> * Nakonfigurované kubectl

Posunutí další kurz toolearn toohello o spuštění aplikace v clusteru hello.

> [!div class="nextstepaction"]
> [Nasazení aplikace v Kubernetes](./container-service-tutorial-kubernetes-deploy-application.md)
