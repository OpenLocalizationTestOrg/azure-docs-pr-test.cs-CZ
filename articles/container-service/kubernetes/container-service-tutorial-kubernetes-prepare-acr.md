---
title: "kurz pro službu kontejneru aaaAzure – Příprava ACR | Microsoft Docs"
description: "Kurz pro Azure Container Service – Příprava ACR"
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
ms.openlocfilehash: 3980e5ce4eb9836f83c761a2f76c944bb3f13060
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-use-azure-container-registry"></a>Nasazení a používání Azure kontejneru registru

Azure kontejneru registru (ACR) je založené na Azure, privátní registru, pro kontejner Docker bitové kopie. Tento kurz, část 7, obě provede nasazení instanci Azure Container registru a předání tooit bitové kopie kontejneru. Dokončit krokům patří:

> [!div class="checklist"]
> * Nasazení instanci Azure Container registru (ACR)
> * Označování bitovou kopii kontejner pro ACR
> * Odesílání bitové kopie tooACR hello

V následujících kurzech této instance ACR je integrovaná do clusteru služby Azure Container Service Kubernetes pro zabezpečené spouštění kontejneru bitové kopie. 

## <a name="before-you-begin"></a>Než začnete

V hello [předchozí kurzu](./container-service-tutorial-kubernetes-prepare-app.md), bitovou kopii kontejner byl vytvořen pro jednoduchou aplikaci Azure hlasování. V tomto kurzu se posune tuto bitovou kopii tooan registru kontejner Azure. Pokud jste dosud nevytvořili hello obrázek aplikace Azure hlasování, vrátí příliš[kurzu 1 – Vytvoření kontejneru image](./container-service-tutorial-kubernetes-prepare-app.md). Alternativně hello kroky podrobné zde pracují se žádný obrázek kontejneru.

Tento kurz vyžaduje, že používáte verzi rozhraní příkazového řádku Azure hello verze 2.0.4 nebo novější. Spustit `az --version` toofind hello verze. Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="deploy-azure-container-registry"></a>Nasadit kontejner Azure registru

Pokud nasazujete registru kontejneru služby Azure, musíte nejprve skupinu prostředků. Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure.

Vytvořte skupinu prostředků s hello [vytvořit skupinu az](/cli/azure/group#create) příkaz. V tomto příkladu skupinu prostředků s názvem *myResourceGroup* je vytvořen v hello *westeurope* oblast.

```azurecli
az group create --name myResourceGroup --location westeurope
```

Vytvoření registru kontejneru Azure s hello [vytvořit az acr](/cli/azure/acr#create) příkaz. název kontejneru registru Hello **musí být jedinečné**.

```azurecli
az acr create --resource-group myResourceGroup --name <acrName> --sku Basic --admin-enabled true
```

V rámci hello zbytek tohoto kurzu používáme "acrname" jako zástupný symbol pro název registru hello kontejneru, který jste si zvolili.

## <a name="container-registry-login"></a>Kontejner registru přihlášení

Musíte se přihlásit v instanci ACR tooyour před odesláním tooit bitové kopie. Použití hello [az acr přihlášení](https://docs.microsoft.com/en-us/cli/azure/acr#login) příkaz toocomplete hello operaci. Je nutné tooprovide hello jedinečný název zadané toohello kontejneru registru, v okamžiku vytvoření.

```azurecli
az acr login --name <acrName>
```

příkaz Hello vrátí zprávu byla úspěšná přihlášení po dokončení.

## <a name="tag-container-images"></a>Značka kontejneru obrázků

Každý kontejner image musí toobe označené hello loginServer název registru hello. Tato značka se používá pro směrování při nabízení kontejneru bitové kopie tooan image registru.

toosee seznam aktuální Image, použijte hello [imagí dockeru](https://docs.docker.com/engine/reference/commandline/images/) příkaz.

```bash
docker images
```

Výstup:

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED             SIZE
azure-vote-front             latest              4675398c9172        13 minutes ago      694MB
redis                        latest              a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask   flask               788ca94b2313        9 months ago        694MB
```

tooget hello loginServer název, spusťte následující příkaz hello.

```azurecli
az acr show --name <acrName> --query loginServer --output table
```

Nyní, značka hello *azure hlas front* bitovou kopii s loginServer hello hello kontejneru registru. Navíc přidat `:redis-v1` toohello konec hello název bitové kopie. Tato značka označuje verzi obrázku hello.

```bash
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:redis-v1
```

Označí, spusťte po [imagí dockeru] (https://docs.docker.com/engine/reference/commandline/images/) tooverify hello operaci.

```bash
docker images
```

Výstup:

```bash
REPOSITORY                                           TAG                 IMAGE ID            CREATED             SIZE
azure-vote-front                                     latest              eaf2b9c57e5e        8 minutes ago       716 MB
mycontainerregistry082.azurecr.io/azure-vote-front   redis-v1            eaf2b9c57e5e        8 minutes ago       716 MB
redis                                                latest              a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask                           flask               788ca94b2313        8 months ago        694 MB
```

## <a name="push-images-tooregistry"></a>Push tooregistry bitové kopie

Push hello *azure hlas front* registru toohello bitové kopie. 

Pomocí následující ukázka hello, nahraďte název loginServer ACR hello hello loginServer ze svého prostředí.

```bash
docker push <acrLoginServer>/azure-vote-front:redis-v1
```

To bude trvat několik minut toocomplete.

## <a name="list-images-in-registry"></a>Seznam obrázků v registru

tooreturn seznam bitové kopie, které se nabídne tooyour kontejneru Azure registru, uživatel hello [az acr úložiště seznamu](/cli/azure/acr/repository#list) příkaz. Aktualizujte hello příkaz s názvem instance ACR hello.

```azurecli
az acr repository list --name <acrName> --output table
```

Výstup:

```azurecli
Result
----------------
azure-vote-front
```

A pak toosee hello značky pro konkrétní image, pomocí hello [az acr úložiště zobrazit značky](/cli/azure/acr/repository#show-tags) příkaz.

```azurecli
az acr repository show-tags --name <acrName> --repository azure-vote-front --output table
```

Výstup:

```azurecli
Result
--------
redis-v1
```

V kurzu dokončení hello kontejneru image byla uložena v privátní instanci Azure Container registru. Tato bitová kopie nasazována z ACR tooa Kubernetes clusteru v následujících kurzech.

## <a name="next-steps"></a>Další kroky

V tomto kurzu registru kontejner Azure připravené pro použití v clusteru služby ACS Kubernetes. byly dokončeny Hello následující kroky:

> [!div class="checklist"]
> * Nasazení instanci Azure Container registru
> * Označí image kontejner pro ACR
> * TooACR image nahrané hello

Posunutí další kurz toolearn toohello o nasazení clusteru s podporou Kubernetes v Azure.

> [!div class="nextstepaction"]
> [Nasazení clusteru Kubernetes](./container-service-tutorial-kubernetes-deploy-cluster.md)