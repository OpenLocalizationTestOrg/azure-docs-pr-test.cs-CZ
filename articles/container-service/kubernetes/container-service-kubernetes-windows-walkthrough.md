---
title: aaaQuickstart - clusteru Azure Kubernetes pro Windows | Microsoft Docs
description: "Naučte se rychle toocreate Kubernetes clusteru Windows kontejnerů v Azure Container Service s hello rozhraní příkazového řádku Azure."
documentationcenter: 
author: dlepow
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
ms.date: 07/18/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: 85fe65a46ae8c78797e8a8a097c2a37f06329335
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-kubernetes-cluster-for-windows-containers"></a>Nasazení clusteru Kubernetes pro kontejnery Windows

Hello rozhraní příkazového řádku Azure je použité toocreate a spravovat prostředky Azure z hello příkazového řádku nebo ve skriptech. Tato příručka podrobně popisuje pomocí rozhraní příkazového řádku Azure toodeploy hello [Kubernetes](https://kubernetes.io/docs/home/) clusteru v [Azure Container Service](../container-service-intro.md). Po nasazení clusteru hello připojíte tooit s hello Kubernetes `kubectl` nástroj příkazového řádku a nasadit vaši první kontejneru systému Windows.

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud zvolíte tooinstall a místně pomocí hello rozhraní příkazového řádku, tento rychlý start vyžaduje, že používáte verzi rozhraní příkazového řádku Azure hello verze 2.0.4 nebo novější. Spustit `az --version` toofind hello verze. Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli). 

> [!NOTE]
> Podpora pro kontejnery Windows v Kubernetes ve službě Azure Container Service je ve verzi Preview. 
>

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte skupinu prostředků s hello [vytvořit skupinu az](/cli/azure/group#create) příkaz. Skupina prostředků Azure je logická skupina, ve které se nasazují a spravují prostředky Azure. 

Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *eastus* umístění.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-kubernetes-cluster"></a>Vytvoření clusteru Kubernetes
Vytvoření clusteru Kubernetes v Azure Container Service s hello [vytvořit acs az](/cli/azure/acs#create) příkaz. 

Hello následující příklad vytvoří cluster s názvem *myK8sCluster* s Linuxem jeden hlavní uzel a dva uzly agenta systému Windows. Tento příklad vytvoří hlavní Linux toohello potřebné tooconnect klíče SSH. Tento příklad používá *azureuser* pro název správce a *myPassword12* jako heslo hello na uzlech Windows hello. Aktualizujte tyto hodnoty toosomething odpovídající tooyour prostředí. 



```azurecli-interactive 
az acs create --orchestrator-type=kubernetes \
    --resource-group myResourceGroup \
    --name=myK8sCluster \
    --agent-count=2 \
    --generate-ssh-keys \
    --windows --admin-username azureuser \
    --admin-password myPassword12
```

Po několika minutách hello příkaz dokončí a zobrazí informace o nasazení.

## <a name="install-kubectl"></a>Instalace kubectl

tooconnect toohello Kubernetes clusteru z klientského počítače, použijte [ `kubectl` ](https://kubernetes.io/docs/user-guide/kubectl/), hello Kubernetes v klientu příkazového řádku. 

Pokud používáte Azure CloudShell, `kubectl` je už nainstalován. Pokud chcete, aby tooinstall ho místně, můžete použít hello [az acs kubernetes instalace rozhraní příkazového řádku](/cli/azure/acs/kubernetes#install-cli) příkaz.

Následující příklad nainstaluje rozhraní příkazového řádku Azure Hello `kubectl` tooyour systému. V systému Windows tento příkaz spusťte jako správce.

```azurecli-interactive 
az acs kubernetes install-cli
```


## <a name="connect-with-kubectl"></a>Připojení přes kubectl

tooconfigure `kubectl` tooconnect tooyour Kubernetes clusteru, spusťte hello [az acs kubernetes get pověření](/cli/azure/acs/kubernetes#get-credentials) příkaz. Hello následující příklad stáhne hello konfigurace clusteru pro váš cluster Kubernetes.

```azurecli-interactive 
az acs kubernetes get-credentials --resource-group=myResourceGroup --name=myK8sCluster
```

tooverify hello připojení tooyour clusteru z počítače, zkuste spustit:

```azurecli-interactive
kubectl get nodes
```

`kubectl`uvádí hello hlavní server a agenta uzly.

```azurecli-interactive
NAME                    STATUS                     AGE       VERSION
k8s-agent-98dc3136-0    Ready                      5m        v1.5.3
k8s-agent-98dc3136-1    Ready                      5m        v1.5.3
k8s-master-98dc3136-0   Ready,SchedulingDisabled   5m        v1.5.3

```

## <a name="deploy-a-windows-iis-container"></a>Nasazení kontejneru Windows služby IIS

Kontejner Dockeru můžete spustit v *podu* Kubernetes, který obsahuje jeden nebo více kontejnerů. 

Tato základní příklad používá toospecify soubor JSON kontejner Microsoft Internet Information Server (IIS) a potom vytvoří hello pod pomocí hello `kubctl apply` příkaz. 

Vytvořte místní soubor s názvem `iis.json` a kopírování hello následující text. Tento soubor informuje Kubernetes toorun služby IIS v systému Windows Server 2016 Nano Server pomocí bitové kopie veřejného kontejneru z [úložiště Docker Hub](https://hub.docker.com/r/nanoserver/iis/). kontejner Hello používá port 80, ale zpočátku je k dispozici pouze v rámci sítě clusteru hello.

 ```JSON
 {
  "apiVersion": "v1",
  "kind": "Pod",
  "metadata": {
    "name": "iis",
    "labels": {
      "name": "iis"
    }
  },
  "spec": {
    "containers": [
      {
        "name": "iis",
        "image": "nanoserver/iis",
        "ports": [
          {
          "containerPort": 80
          }
        ]
      }
    ],
    "nodeSelector": {
     "beta.kubernetes.io/os": "windows"
     }
   }
 }
 ```

toostart hello pod, typ:
  
```azurecli-interactive
kubectl apply -f iis.json
```  

tootrack hello nasazení, typ:
  
```azurecli-interactive
kubectl get pods
```

Hello pod je nasazení, i když je stav hello `ContainerCreating`. Může trvat několik minut, než hello kontejneru tooenter hello `Running` stavu.

```azurecli-interactive
NAME     READY        STATUS        RESTARTS    AGE
iis      1/1          Running       0           32s
```

## <a name="view-hello-iis-welcome-page"></a>Zobrazení hello úvodní stránka služby IIS

tooexpose pod toohello Vítáme s veřejnou IP adresu, typ hello následující příkaz:

```azurecli-interactive
kubectl expose pods iis --port=80 --type=LoadBalancer
```

Pomocí tohoto příkazu Kubernetes vytvoří službu a [pravidlo nástroje pro vyrovnávání zatížení Azure](container-service-kubernetes-load-balancing.md) s veřejnou IP adresu pro službu hello. 

Spusťte následující příkaz toosee hello stav služby hello hello.

```azurecli-interactive
kubectl get svc
```

Původně hello IP adresa se zobrazí jako `pending`. Po několika minutách hello externí IP adresu hello `iis` pod nastavena:
  
```azurecli-interactive
NAME         CLUSTER-IP     EXTERNAL-IP     PORT(S)        AGE       
kubernetes   10.0.0.1       <none>          443/TCP        21h       
iis          10.0.111.25    13.64.158.233   80/TCP         22m
```

Můžete použít webový prohlížeč choice toosee hello výchozí IIS úvodní stránky v hello externí IP adresu:

![Obrázek procházení tooIIS](./media/container-service-kubernetes-windows-walkthrough/kubernetes-iis.png)  


## <a name="delete-cluster"></a>Odstranění clusteru
Pokud hello cluster je již nepotřebujete, můžete použít hello [odstranění skupiny az](/cli/azure/group#delete) příkaz skupiny prostředků hello tooremove, container service a všechny související prostředky.

```azurecli-interactive 
az group delete --name myResourceGroup
```


## <a name="next-steps"></a>Další kroky

V tomto rychlém úvodním kurzu jste nasadili cluster Kubernetes, připojili se přes `kubectl` a nasadili pod s kontejnerem ISS. toolearn Další informace o Azure Container Service pokračovat toohello Kubernetes kurzu.

> [!div class="nextstepaction"]
> [Správa clusteru ACS Kubernetes](container-service-tutorial-kubernetes-prepare-app.md)
