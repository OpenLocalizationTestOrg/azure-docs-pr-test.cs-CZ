---
title: "kurz instancí kontejnerů aaaAzure – Příprava registru kontejner Azure | Microsoft Docs"
description: "Kurz pro Azure instancí kontejnerů – Příprava registru kontejner Azure"
services: container-instances
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, Kontejnery, mikroslužby, Kubernetes, DC/OS, Azure"
ms.assetid: 
ms.service: container-instances
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 2525626125740c3c861fad36aad207d0b587ff54
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-use-azure-container-registry"></a>Nasazení a používání Azure kontejneru registru

Toto je část dva kurzu třemi částmi. V hello [předchozího kroku](./container-instances-tutorial-prepare-app.md), bitovou kopii kontejner byl vytvořen pro jednoduché webové aplikace napsané v [Node.js](http://nodejs.org). V tomto kurzu se posune tuto bitovou kopii tooan registru kontejner Azure. Pokud jste dosud nevytvořili hello kontejneru image, vrátí příliš[kurzu 1 – Vytvoření kontejneru image](./container-instances-tutorial-prepare-app.md). 

Hello registru kontejner Azure je založené na Azure, privátní registru, pro kontejner Docker bitové kopie. Tento kurz vás provede nasazení instanci Azure Container registru a předání tooit bitové kopie kontejneru. Dokončit krokům patří:

> [!div class="checklist"]
> * Nasazení instanci Azure Container registru
> * Označování kontejneru image pro registru kontejner Azure
> * Odesílání bitové kopie tooAzure registru kontejneru

V následujících kurzech nasadit hello kontejneru z vaší privátní registru tooAzure instancí kontejnerů.

## <a name="before-you-begin"></a>Než začnete

Tento kurz vyžaduje, že používáte verzi rozhraní příkazového řádku Azure hello verze 2.0.4 nebo novější. Spustit `az --version` toofind hello verze. Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).

## <a name="deploy-azure-container-registry"></a>Nasadit kontejner Azure registru

Pokud nasazujete registru kontejneru služby Azure, musíte nejprve skupinu prostředků. Skupinu prostředků Azure je logické kolekce do Azure, které jsou nasazené a spravované prostředky.

Vytvořte skupinu prostředků s hello [vytvořit skupinu az](/cli/azure/group#create) příkaz. V tomto příkladu skupinu prostředků s názvem *myResourceGroup* je vytvořen v hello *eastus* oblast.

```azurecli
az group create --name myResourceGroup --location eastus
```

Vytvoření registru kontejneru Azure s hello [vytvořit az acr](/cli/azure/acr#create) příkaz. název kontejneru registru Hello **musí být jedinečné**. V následující ukázka hello, použijeme hello název *mycontainerregistry082*.

```azurecli
az acr create --resource-group myResourceGroup --name mycontainerregistry082 --sku Basic --admin-enabled true
```

V rámci hello zbytek v tomto kurzu používáme `<acrname>` jako zástupný symbol pro název registru hello kontejneru, který jste si zvolili.

## <a name="container-registry-login"></a>Kontejner registru přihlášení

Musíte se přihlásit v instanci ACR tooyour před odesláním tooit bitové kopie. Použití hello [az acr přihlášení](https://docs.microsoft.com/en-us/cli/azure/acr#login) příkaz toocomplete hello operaci. Je nutné tooprovide hello jedinečný název zadané toohello kontejneru registru, v okamžiku vytvoření.

```azurecli
az acr login --name <acrName>
```

příkaz Hello vrátí zprávu byla úspěšná přihlášení po dokončení.

## <a name="tag-container-image"></a>Obrázek značky kontejneru

toodeploy kontejneru image z privátní registru hello image musí toobe označené hello `loginServer` název registru hello.

toosee seznam aktuální Image, použijte hello `docker images` příkaz.

```bash
docker images
```

Výstup:

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED              SIZE
aci-tutorial-app             latest              5c745774dfa9        39 seconds ago       68.1 MB
```

tooget hello loginServer název, spusťte následující příkaz hello.

```azurecli
az acr show --name <acrName> --query loginServer --output table
```

Značka hello *aci – kurz aplikace* bitovou kopii s loginServer hello hello kontejneru registru. Navíc přidat `:v1` toohello konec hello název bitové kopie. Tato značka označuje číslo verze hello bitové kopie.

```bash
docker tag aci-tutorial-app <acrLoginServer>/aci-tutorial-app:v1
```

Jakmile příznakem, spustit `docker images` tooverify hello operaci.

```bash
docker images
```

Výstup:

```bash
REPOSITORY                                                TAG                 IMAGE ID            CREATED             SIZE
aci-tutorial-app                                          latest              5c745774dfa9        39 seconds ago      68.1 MB
mycontainerregistry082.azurecr.io/aci-tutorial-app        v1                  a9dace4e1a17        7 minutes ago       68.1 MB
```

## <a name="push-image-tooazure-container-registry"></a>Push image tooAzure registru kontejneru

Push hello *aci – kurz aplikace* registru toohello bitové kopie.

Pomocí následující ukázka hello, nahraďte název loginServer registru kontejneru hello hello loginServer ze svého prostředí.

```bash
docker push <acrLoginServer>/aci-tutorial-app:v1
```

## <a name="list-images-in-azure-container-registry"></a>Seznam obrázků v registru kontejner Azure

tooreturn seznam bitové kopie, které se nabídne tooyour kontejneru Azure registru, uživatel hello [az acr úložiště seznamu](/cli/azure/acr/repository#list) příkaz. Aktualizujte hello příkaz s názvem registru hello kontejneru.

```azurecli
az acr repository list --name <acrName> --output table
```

Výstup:

```azurecli
Result
----------------
aci-tutorial-app
```

A pak toosee hello značky pro konkrétní image, pomocí hello [az acr úložiště zobrazit značky](/cli/azure/acr/repository#show-tags) příkaz.

```azurecli
az acr repository show-tags --name <acrName> --repository aci-tutorial-app --output table
```

Výstup:

```azurecli
Result
--------
v1
```

## <a name="next-steps"></a>Další kroky

V tomto kurzu registru kontejner Azure připravené pro použití s instancemi Azure kontejneru a byla posunuta hello kontejneru image. byly dokončeny Hello následující kroky:

> [!div class="checklist"]
> * Nasazení instanci Azure Container registru
> * Označování kontejneru image pro registru kontejner Azure
> * Odesílání bitové kopie tooAzure registru kontejneru

Posunutí další kurz toolearn toohello o nasazení tooAzure kontejneru hello pomocí Azure kontejner instancí.

> [!div class="nextstepaction"]
> [Nasazení kontejnerů tooAzure instancí kontejnerů](./container-instances-tutorial-deploy-app.md)
