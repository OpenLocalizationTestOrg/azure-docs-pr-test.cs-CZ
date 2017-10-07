---
title: aaaQuickstart - Azure Kubernetes clusteru pro Linux | Microsoft Docs
description: "Naučte se rychle toocreate cluster Kubernetes pro Linux kontejnerů v Azure Container Service pomocí hello rozhraní příkazového řádku Azure."
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.assetid: 8da267e8-2aeb-4c24-9a7a-65bdca3a82d6
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/21/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: 8b0d7a803148c1cbf329f4b76f2e99b4b7e14983
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-kubernetes-cluster-for-linux-containers"></a>Nasazení clusteru Kubernetes pro linuxové kontejnery

V této úvodní Kubernetes cluster nasadí pomocí hello rozhraní příkazového řádku Azure. Aplikace více kontejneru, který se skládá z webového front-endu a instanci Redis nasazení a poté běží na clusteru hello. Po dokončení aplikace hello je přístupné prostřednictvím Internetu hello. 

Ukázková aplikace Hello v tomto dokumentu je napsán v Python. Koncepty Hello a kroky popsané v tomto poli můžou být použité toodeploy kontejneru, bitové kopie do clusteru s podporou Kubernetes. Hello kód, soubor Docker a předem vytvořené Kubernetes souborů manifestu související toothis projektu jsou k dispozici na [Githubu](https://github.com/Azure-Samples/azure-voting-app-redis.git).

![Obrázek procházení tooAzure hlas](media/container-service-kubernetes-walkthrough/azure-vote.png)

Tento úvodní předpokládá základní znalosti koncepce Kubernetes najdete podrobné informace o Kubernetes hello [Kubernetes dokumentaci]( https://kubernetes.io/docs/home/).

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud zvolíte tooinstall a místně pomocí hello rozhraní příkazového řádku, tento rychlý start vyžaduje, že používáte verzi rozhraní příkazového řádku Azure hello verze 2.0.4 nebo novější. Spustit `az --version` toofind hello verze. Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte skupinu prostředků s hello [vytvořit skupinu az](/cli/azure/group#create) příkaz. Skupina prostředků Azure je logická skupina, ve které se nasazují a spravují prostředky Azure. 

Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *westeurope* umístění.

```azurecli-interactive 
az group create --name myResourceGroup --location westeurope
```

Výstup:

```json
{
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup",
  "location": "westeurope",
  "managedBy": null,
  "name": "myResourceGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

## <a name="create-kubernetes-cluster"></a>Vytvoření clusteru Kubernetes

Vytvoření clusteru Kubernetes v Azure Container Service s hello [vytvořit acs az](/cli/azure/acs#create) příkaz. Hello následující příklad vytvoří cluster s názvem *myK8sCluster* s Linuxem jeden hlavní uzel a tři uzly Linux agent.

```azurecli-interactive 
az acs create --orchestrator-type kubernetes --resource-group myResourceGroup --name myK8sCluster --generate-ssh-keys 
```

Po několika minutách hello příkaz dokončí a vrátí formátu json informace o clusteru hello. 

## <a name="connect-toohello-cluster"></a>Připojte toohello cluster

použít toomanage Kubernetes cluster [kubectl](https://kubernetes.io/docs/user-guide/kubectl/), hello Kubernetes v klientu příkazového řádku. 

Pokud používáte Azure Cloud Shell, kubectl je už nainstalován. Pokud chcete, aby tooinstall ho místně, můžete použít hello [az acs kubernetes instalace rozhraní příkazového řádku](/cli/azure/acs/kubernetes#install-cli) příkaz.

tooconfigure kubectl tooconnect tooyour Kubernetes clusteru, spusťte hello [az acs kubernetes get pověření](/cli/azure/acs/kubernetes#get-credentials) příkaz. Tento krok stáhne přihlašovací údaje a nakonfiguruje hello toouse Kubernetes rozhraní příkazového řádku je.

```azurecli-interactive 
az acs kubernetes get-credentials --resource-group=myResourceGroup --name=myK8sCluster
```

tooverify hello připojení tooyour clusteru, použijte hello [kubectl získat](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) příkaz tooreturn seznam hello uzly clusteru.

```azurecli-interactive
kubectl get nodes
```

Výstup:

```bash
NAME                    STATUS                     AGE       VERSION
k8s-agent-14ad53a1-0    Ready                      10m       v1.6.6
k8s-agent-14ad53a1-1    Ready                      10m       v1.6.6
k8s-agent-14ad53a1-2    Ready                      10m       v1.6.6
k8s-master-14ad53a1-0   Ready,SchedulingDisabled   10m       v1.6.6
```

## <a name="run-hello-application"></a>Spuštění aplikace hello

Soubor manifestu Kubernetes definuje požadovaný stav hello clusteru, včetně toho, co by měla být spuštěná kontejneru bitové kopie. V tomto příkladu je manifestu použité toocreate všechny objekty potřeby toorun hello hlas Azure aplikace. 

Vytvořte soubor s názvem `azure-vote.yml` a zkopírujte do něj následující YAML hello. Pokud pracujete ve službě Azure Cloud Shell, můžete tento soubor vytvořit pomocí editoru vi nebo Nano stejně, jako kdybyste pracovali na virtuálním nebo fyzickém systému.

```yaml
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: azure-vote-back
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: azure-vote-back
    spec:
      containers:
      - name: azure-vote-back
        image: redis
        ports:
        - containerPort: 6379
          name: redis
---
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-back
spec:
  ports:
  - port: 6379
  selector:
    app: azure-vote-back
---
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: azure-vote-front
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: azure-vote-front
    spec:
      containers:
      - name: azure-vote-front
        image: microsoft/azure-vote-front:redis-v1
        ports:
        - containerPort: 80
        env:
        - name: REDIS
          value: "azure-vote-back"
---
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-front
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-vote-front
```

Použití hello [kubectl vytvořit](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#create) příkaz toorun hello aplikace.

```azurecli-interactive
kubectl create -f azure-vote.yml
```

Výstup:

```bash
deployment "azure-vote-back" created
service "azure-vote-back" created
deployment "azure-vote-front" created
service "azure-vote-front" created
```

## <a name="test-hello-application"></a>Testování aplikace hello

Jako hello aplikace běží, [Kubernetes služby](https://kubernetes.io/docs/concepts/services-networking/service/) se vytvoří, zpřístupňuje hello aplikace front-endu toohello Internetu. Tento proces může trvat několik minut toocomplete. 

toomonitor průběh, použijte hello [kubectl získat služby](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) s hello `--watch` argument.

```azurecli-interactive
kubectl get service azure-vote-front --watch
```

Původně hello **externí IP** pro hello *azure hlas front* služby se zobrazí jako *čekající*. Jakmile hello externí IP adresu se změnil z *čekající* tooan *IP adresu*, použijte `CTRL-C` toostop hello kubectl sledovat proces. 
  
```bash
azure-vote-front   10.0.34.242   <pending>     80:30676/TCP   7s
azure-vote-front   10.0.34.242   52.179.23.131   80:30676/TCP   2m
```

Nyní je možné procházet toohello externí IP adresu toosee hello hlas aplikace Azure.

![Obrázek procházení tooAzure hlas](media/container-service-kubernetes-walkthrough/azure-vote.png)  

## <a name="delete-cluster"></a>Odstranění clusteru
Pokud hello cluster je již nepotřebujete, můžete použít hello [odstranění skupiny az](/cli/azure/group#delete) příkaz skupiny prostředků hello tooremove, container service a všechny související prostředky.

```azurecli-interactive 
az group delete --name myResourceGroup --yes --no-wait
```

## <a name="get-hello-code"></a>Získat kód hello

V této úvodní předem vytvořené kontejneru bitové kopie byly použité toocreate Kubernetes nasazení. Hello související kód aplikace, soubor Docker, a soubor manifestu Kubernetes jsou dostupné na Githubu.

[https://github.com/Azure-Samples/azure-voting-app-redis](https://github.com/Azure-Samples/azure-voting-app-redis.git)

## <a name="next-steps"></a>Další kroky

V této úvodní nasazení clusteru s podporou Kubernetes a nasadit aplikace s více kontejnerů tooit. 

toolearn informace o Azure Container Service a pomocí příklad toodeployment dokončení kódu, pokračovat toohello Kubernetes clusteru kurzu.

> [!div class="nextstepaction"]
> [Správa clusteru ACS Kubernetes](./container-service-tutorial-kubernetes-prepare-app.md)
