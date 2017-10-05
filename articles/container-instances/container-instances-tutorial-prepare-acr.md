---
title: "Kurz pro Azure instancí kontejnerů – Příprava registru kontejner Azure | Microsoft Docs"
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
ms.openlocfilehash: cc96ba9f5abd45a7503ba3327b30e1f809391384
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="deploy-and-use-azure-container-registry"></a>Nasazení a používání Azure kontejneru registru

Toto je část dva kurzu třemi částmi. V [předchozího kroku](./container-instances-tutorial-prepare-app.md), bitovou kopii kontejner byl vytvořen pro jednoduché webové aplikace napsané v [Node.js](http://nodejs.org). V tomto kurzu se posune tuto bitovou kopii registru kontejner Azure. Pokud jste ještě nevytvořili bitovou kopii kontejneru, vrátit [kurzu 1 – Vytvoření kontejneru image](./container-instances-tutorial-prepare-app.md). 

Registr kontejner Azure je založené na Azure, privátní registru, pro kontejner Docker bitové kopie. Tento kurz vás provede nasazení instanci Azure Container registru a když zavedete bitovou kopii kontejneru. Dokončit krokům patří:

> [!div class="checklist"]
> * Nasazení instanci Azure Container registru
> * Označování kontejneru image pro registru kontejner Azure
> * Odesílání bitové kopie do registru kontejner Azure

V následujících kurzech můžete nasadit kontejner z vaší privátní registru pro kontejner instancí Azure.

## <a name="before-you-begin"></a>Než začnete

Tento kurz vyžaduje, že používáte Azure CLI verze verze 2.0.4 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).

## <a name="deploy-azure-container-registry"></a>Nasadit kontejner Azure registru

Pokud nasazujete registru kontejneru služby Azure, musíte nejprve skupinu prostředků. Skupinu prostředků Azure je logické kolekce do Azure, které jsou nasazené a spravované prostředky.

Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group#create). V tomto příkladu skupinu prostředků s názvem *myResourceGroup* je vytvořen v *eastus* oblast.

```azurecli
az group create --name myResourceGroup --location eastus
```

Vytvoření kontejneru Azure registr s využitím [az acr vytvořit](/cli/azure/acr#create) příkaz. Název kontejneru registru **musí být jedinečné**. V následujícím příkladu použijeme název *mycontainerregistry082*.

```azurecli
az acr create --resource-group myResourceGroup --name mycontainerregistry082 --sku Basic --admin-enabled true
```

Po celý zbytek v tomto kurzu používáme `<acrname>` jako zástupný symbol pro název kontejneru registru, který jste zvolili.

## <a name="container-registry-login"></a>Kontejner registru přihlášení

Musíte se přihlásit k vaší instanci ACR před odesláním bitové kopie do ní. Použití [az acr přihlášení](https://docs.microsoft.com/en-us/cli/azure/acr#login) příkaz k dokončení operace. Je třeba zadat jedinečný název zadané registru kontejneru v okamžiku vytvoření.

```azurecli
az acr login --name <acrName>
```

Příkaz vrátí zprávu, byla úspěšná přihlášení po dokončení.

## <a name="tag-container-image"></a>Obrázek značky kontejneru

K nasazení bitové kopie kontejneru z privátní registru, musí být označené bitovou kopii `loginServer` název registru.

Chcete-li zobrazit seznam aktuální bitové kopie, použijte `docker images` příkaz.

```bash
docker images
```

Výstup:

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED              SIZE
aci-tutorial-app             latest              5c745774dfa9        39 seconds ago       68.1 MB
```

Získat název loginServer, spusťte následující příkaz.

```azurecli
az acr show --name <acrName> --query loginServer --output table
```

Značka *aci – kurz aplikace* bitovou kopii s loginServer registru kontejneru. Navíc přidat `:v1` na konec název bitové kopie. Tato značka označuje číslo verze bitové kopie.

```bash
docker tag aci-tutorial-app <acrLoginServer>/aci-tutorial-app:v1
```

Jakmile příznakem, spustit `docker images` ověření operaci.

```bash
docker images
```

Výstup:

```bash
REPOSITORY                                                TAG                 IMAGE ID            CREATED             SIZE
aci-tutorial-app                                          latest              5c745774dfa9        39 seconds ago      68.1 MB
mycontainerregistry082.azurecr.io/aci-tutorial-app        v1                  a9dace4e1a17        7 minutes ago       68.1 MB
```

## <a name="push-image-to-azure-container-registry"></a>Push bitové kopie do registru kontejner Azure

Push *aci – kurz aplikace* bitovou kopii do registru.

Pomocí následujícího příkladu, nahraďte název kontejneru registru loginServer loginServer ze svého prostředí.

```bash
docker push <acrLoginServer>/aci-tutorial-app:v1
```

## <a name="list-images-in-azure-container-registry"></a>Seznam obrázků v registru kontejner Azure

K zobrazení seznamu bitové kopie, které se nabídne do vašeho kontejneru Azure registru uživatele [az acr úložiště seznamu](/cli/azure/acr/repository#list) příkaz. Aktualizujte příkaz s názvem registru kontejneru.

```azurecli
az acr repository list --name <acrName> --output table
```

Výstup:

```azurecli
Result
----------------
aci-tutorial-app
```

A pak najdete v části značky pro konkrétní image, pomocí [az acr úložiště zobrazit značky](/cli/azure/acr/repository#show-tags) příkaz.

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

V tomto kurzu registru kontejner Azure připravené pro použití s instancemi Azure kontejneru a bitovou kopii kontejneru se instaluje. Dokončili jste následující kroky:

> [!div class="checklist"]
> * Nasazení instanci Azure Container registru
> * Označování kontejneru image pro registru kontejner Azure
> * Odesílání bitové kopie do registru kontejner Azure

Přechodu na další kurzu se dozvíte o nasazení do Azure pomocí Azure kontejner instancí kontejneru.

> [!div class="nextstepaction"]
> [Kontejnery můžete nasadit do Azure kontejner instancí](./container-instances-tutorial-deploy-app.md)
