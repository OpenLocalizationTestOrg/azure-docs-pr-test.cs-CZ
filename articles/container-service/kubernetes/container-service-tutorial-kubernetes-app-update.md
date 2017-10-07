---
title: "kurz pro službu kontejneru aaaAzure – aktualizace aplikací | Microsoft Docs"
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
ms.openlocfilehash: c467498bab7952926a18e45ffbb21051a98739d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="update-an-application-in-kubernetes"></a>Aktualizace aplikace v Kubernetes

Po nasazení aplikace v Kubernetes, můžete aktualizovat tak, že zadáte novou bitovou kopii kontejneru nebo verze bitové kopie. Při aktualizaci aplikace zavedení aktualizace hello je vynášené, aby pouze část nasazení hello souběžně se aktualizuje. Tato dvoufázové instalace aktualizace umožňuje tookeep aplikace hello během aktualizace hello a poskytuje mechanismus vrácení zpět, pokud dojde k selhání nasazení. 

V tomto kurzu se aktualizuje půl součástí sedm, hello ukázkovou aplikaci Azure hlas. Úlohy, které zahrnují:

> [!div class="checklist"]
> * Aktualizace kód aplikace front-endu hello
> * Vytvoření bitové kopie aktualizované kontejneru
> * Vkládání hello kontejneru image tooAzure registru kontejneru
> * Nasazení bitové kopie hello aktualizované kontejneru

V následujících kurzech Operations Management Suite je nakonfigurované toomonitor hello Kubernetes clusteru.

## <a name="before-you-begin"></a>Než začnete

V předchozí kurzy byla aplikace zabalené do kontejneru image, hello image nahráli tooAzure registru kontejneru a cluster Kubernetes vytvořit. Hello aplikace pak byl na hello Kubernetes clusteru spusťte. 

Pokud jste nedokončili tyto kroky a chcete toofollow společně, vrátí příliš[kurzu 1 – Vytvoření kontejneru image](./container-service-tutorial-kubernetes-prepare-app.md). 

## <a name="update-application"></a>Aktualizace aplikace

toocomplete hello kroky v tomto kurzu, budete musí mít klonovat kopii hello hlas Azure aplikace. V případě potřeby vytvořte tato klonovaný kopie s hello následující příkaz:

```bash
git clone https://github.com/Azure-Samples/azure-voting-app-redis.git
```

Otevřete hello `config_file.cfg` soubor pomocí libovolného editoru kódu nebo text. Tento soubor v následujícím adresáři hello klonovat úložiště hello můžete najít.

```bash
 /azure-voting-app-redis/azure-vote/azure-vote/config_file.cfg
```

Změnit hodnoty hello `VOTE1VALUE` a `VOTE2VALUE`a pak hello soubor uložte.

```bash
# UI Configurations
TITLE = 'Azure Voting App'
VOTE1VALUE = 'Blue'
VOTE2VALUE = 'Purple'
SHOWHOST = 'false'
```

Použití [docker compose](https://docs.docker.com/compose/) toore-vytvořit hello front-end bitové kopie a spuštění hello aktualizovat aplikaci.

```bash
docker-compose -f ./azure-voting-app-redis/docker-compose.yml up --build -d
```

## <a name="test-application-locally"></a>Testovací aplikace místně

Procházet příliš`http://localhost:8080` toosee hello aktualizovat aplikaci.

![Obrázek clusteru Kubernetes v Azure](media/container-service-kubernetes-tutorials/vote-app-updated.png)

## <a name="tag-and-push-images"></a>Značky a nabízených bitové kopie

Značka hello *azure hlas front* bitovou kopii s loginServer hello hello kontejneru registru.

Pokud používáte Azure kontejneru registru, získat název serveru hello přihlášení s hello [az acr seznamu](/cli/azure/acr#list) příkaz.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Použití [docker značka](https://docs.docker.com/engine/reference/commandline/tag/) tootag hello image. Nahraďte `<acrLoginServer>` se název serveru registru kontejner Azure přihlášení nebo registru veřejný název hostitele.

```bash
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:redis-v2
```

Použití [docker nabízené](https://docs.docker.com/engine/reference/commandline/push/) tooupload hello image tooyour registru. Nahraďte `<acrLoginServer>` se název serveru registru kontejner Azure přihlášení nebo registru veřejný název hostitele.

```bash
docker push <acrLoginServer>/azure-vote-front:redis-v2
```

## <a name="deploy-update-application"></a>Nasazení aktualizace aplikace

Maximální doba provozu tooensure, musí být spuštěna více instancí pod aplikace hello. Tuto konfiguraci ověříte s hello [kubectl získat pod](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) příkaz.

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

Pokud nemáte k dispozici více pracovními stanicemi soustředěnými kolem systémem azure hlas front hello bitové kopie, škálovat hello *azure hlas front* nasazení.


```azurecli-interactive
kubectl scale --replicas=3 deployment/azure-vote-front
```

tooupdate hello aplikace, použijte hello [kubectl sady](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#set) příkaz. Aktualizace `<acrLoginServer>` s hello přihlášení serveru nebo název hostitele vašeho kontejneru registru.

```azurecli-interactive
kubectl set image deployment azure-vote-front azure-vote-front=<acrLoginServer>/azure-vote-front:redis-v2
```

toomonitor hello nasazení, použijte hello [kubectl získat pod](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) příkaz. Jako hello aktualizovat aplikace nasazena, jsou vaše pracovními stanicemi soustředěnými kolem ukončena a znovu vytvořit s hello novou bitovou kopii kontejneru.

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

Získat hello externí IP adresu hello *azure hlas front* služby.

```azurecli-interactive
kubectl get service azure-vote-front
```

Procházet toohello IP adresu toosee hello aktualizovat aplikaci.

![Obrázek clusteru Kubernetes v Azure](media/container-service-kubernetes-tutorials/vote-app-updated-external.png)

## <a name="next-steps"></a>Další kroky

V tomto kurzu aktualizovat aplikaci a nasazuje clusteru Kubernetes tooa této aktualizace. byly dokončeny Hello následující úlohy:

> [!div class="checklist"]
> * Kód aplikace front-endu aktualizované hello
> * Vytvořit bitovou kopii aktualizované kontejneru
> * Poslat hello kontejneru image tooAzure registru kontejneru
> * Aplikace nasazené hello aktualizovat

Posunutí další kurz toolearn toohello o toomonitor Kubernetes s Operations Management Suite.

> [!div class="nextstepaction"]
> [Monitorování Kubernetes pomocí OMS](./container-service-tutorial-kubernetes-monitor.md)