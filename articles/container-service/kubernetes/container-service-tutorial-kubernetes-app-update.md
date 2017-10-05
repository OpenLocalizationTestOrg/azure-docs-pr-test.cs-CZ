---
title: "Kurz pro Azure Container Service – aktualizace aplikací | Microsoft Docs"
description: "Kurz pro Azure Container Service – aktualizace aplikace"
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
ms.date: 07/26/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: db580da3e2d70892bc37305394df5be609ebb8a3
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="update-an-application-in-kubernetes"></a>Aktualizace aplikace v Kubernetes

Po nasazení aplikace v Kubernetes, můžete aktualizovat tak, že zadáte novou bitovou kopii kontejneru nebo verze bitové kopie. Při aktualizaci aplikace je zavedení aktualizace připraveny tak, aby souběžně se aktualizuje pouze část nasazení. Tato dvoufázové instalace aktualizace umožňuje aplikaci běžet během aktualizace a poskytuje mechanismus vrácení zpět, pokud dojde k selhání nasazení. 

V tomto kurzu, součástí půl 7, se aktualizuje ukázkovou aplikaci Azure hlas. Úlohy, které zahrnují:

> [!div class="checklist"]
> * Aktualizace kód aplikace front-endu
> * Vytvoření bitové kopie aktualizované kontejneru
> * Když zavedete bitovou kopii kontejneru registru kontejner Azure
> * Nasazení bitové kopie aktualizované kontejneru

V následujících kurzech Operations Management Suite nakonfigurován ke sledování Kubernetes clusteru.

## <a name="before-you-begin"></a>Než začnete

V předchozích kurzech byla aplikace zabalené do kontejneru image, image nahrané do registru kontejner Azure a cluster Kubernetes vytvořili. Aplikace pak byl na Kubernetes clusteru spusťte. 

Pokud jste nedokončili tyto kroky a chcete sledovat, vrátit [kurzu 1 – Vytvoření kontejneru image](./container-service-tutorial-kubernetes-prepare-app.md). 

## <a name="update-application"></a>Aktualizace aplikace

Pokud chcete provést kroky v tomto kurzu, musí mít klonovat kopie aplikace Azure hlas. V případě potřeby vytvořte tato klonovaný kopie pomocí následujícího příkazu:

```bash
git clone https://github.com/Azure-Samples/azure-voting-app-redis.git
```

Otevřete `config_file.cfg` soubor pomocí libovolného editoru kódu nebo text. Můžete najít tento soubor v následujícím adresáři klonovaný úložišti.

```bash
 /azure-voting-app-redis/azure-vote/azure-vote/config_file.cfg
```

Změnit hodnoty nastavení `VOTE1VALUE` a `VOTE2VALUE`a pak soubor uložte.

```bash
# UI Configurations
TITLE = 'Azure Voting App'
VOTE1VALUE = 'Blue'
VOTE2VALUE = 'Purple'
SHOWHOST = 'false'
```

Použití [docker compose](https://docs.docker.com/compose/) znovu vytvořit bitovou kopii front-endu a spustit aktualizovanou aplikaci.

```bash
docker-compose -f ./azure-voting-app-redis/docker-compose.yml up --build -d
```

## <a name="test-application-locally"></a>Testovací aplikace místně

Přejděte do `http://localhost:8080` zobrazíte aktualizovanou aplikaci.

![Obrázek clusteru Kubernetes v Azure](media/container-service-kubernetes-tutorials/vote-app-updated.png)

## <a name="tag-and-push-images"></a>Značky a nabízených bitové kopie

Značka *azure hlas front* bitovou kopii s loginServer registru kontejneru.

Pokud používáte Azure kontejneru registru, získat přihlašovací jméno pro server s [az acr seznamu](/cli/azure/acr#list) příkaz.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Použití [docker značky](https://docs.docker.com/engine/reference/commandline/tag/) k označení bitovou kopii. Nahraďte `<acrLoginServer>` se název serveru registru kontejner Azure přihlášení nebo registru veřejný název hostitele.

```bash
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:redis-v2
```

Použití [docker nabízené](https://docs.docker.com/engine/reference/commandline/push/) Odeslat bitovou kopii do registru. Nahraďte `<acrLoginServer>` se název serveru registru kontejner Azure přihlášení nebo registru veřejný název hostitele.

```bash
docker push <acrLoginServer>/azure-vote-front:redis-v2
```

## <a name="deploy-update-application"></a>Nasazení aktualizace aplikace

Aby maximální doba provozu, musí být spuštěna více instancí pod aplikace. Ověřte, tato konfigurace se [kubectl získat pod](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) příkaz.

```bash
kubectl get pod
```

Výstup:

```bash
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-217588096-5w632    1/1       Running   0          10m
azure-vote-front-233282510-b5pkz   1/1       Running   0          10m
azure-vote-front-233282510-dhrtr   1/1       Running   0          10m
azure-vote-front-233282510-pqbfk   1/1       Running   0          10m
```

Pokud nemáte k dispozici více pracovními stanicemi soustředěnými kolem systémem bitovou kopii azure hlas front, škálování *azure hlas front* nasazení.


```azurecli-interactive
kubectl scale --replicas=3 deployment/azure-vote-front
```

Chcete-li aktualizovat aplikace, použijte [kubectl sady](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#set) příkaz. Aktualizace `<acrLoginServer>` s přihlášení serveru nebo název hostitele vašeho kontejneru registru.

```azurecli-interactive
kubectl set image deployment azure-vote-front azure-vote-front=<acrLoginServer>/azure-vote-front:redis-v2
```

Ke sledování nasazení, použijte [kubectl získat pod](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) příkaz. Jako aktualizované aplikace nasazena, jsou vaše pracovními stanicemi soustředěnými kolem ukončena a znovu vytvořit s novou bitovou kopii kontejneru.

```azurecli-interactive
kubectl get pod
```

Výstup:

```bash
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-2978095810-gq9g0   1/1       Running   0          5m
azure-vote-front-1297194256-tpjlg   1/1       Running   0         1m
azure-vote-front-1297194256-tptnx   1/1       Running   0         5m
azure-vote-front-1297194256-zktw9   1/1       Terminating   0         1m
```

## <a name="test-updated-application"></a>Testovací aktualizovat aplikaci

Získat externí IP adresu *azure hlas front* služby.

```azurecli-interactive
kubectl get service azure-vote-front
```

Přejděte na IP adresu, kterou najdete v části aktualizovanou aplikaci.

![Obrázek clusteru Kubernetes v Azure](media/container-service-kubernetes-tutorials/vote-app-updated-external.png)

## <a name="next-steps"></a>Další kroky

V tomto kurzu aktualizovat aplikaci a vrátit se tato aktualizace na clusteru s podporou Kubernetes. Byly dokončit následující úlohy:

> [!div class="checklist"]
> * Aktualizovat kód aplikace front-endu
> * Vytvořit bitovou kopii aktualizované kontejneru
> * Nainstaluje bitovou kopii kontejneru do registru kontejner Azure
> * Aktualizovaná aplikace nasazena

Přechodu na v dalším kurzu se dozvíte o tom, jak sledovat Kubernetes s Operations Management Suite.

> [!div class="nextstepaction"]
> [Monitorování Kubernetes pomocí OMS](./container-service-tutorial-kubernetes-monitor.md)