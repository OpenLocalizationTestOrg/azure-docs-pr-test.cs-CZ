---
title: "aaaCreate privátní registru kontejner Docker - rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Začínáme vytváření a správě privátního registrech kontejner Docker s hello 2.0 rozhraní příkazového řádku Azure"
services: container-registry
documentationcenter: 
author: neilpeterson
manager: timlt
editor: na
tags: 
keywords: 
ms.assetid: 29e20d75-bf39-4f7d-815f-a2e47209be7d
ms.service: container-registry
ms.devlang: azurecli
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/11/2017
ms.author: nepeters
ms.custom: na
ms.openlocfilehash: 2cadf42db0681a09c95486510f1e65c6f87c5280
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-managed-container-registry-using-hello-azure-cli"></a>Vytvoření spravované kontejneru registru pomocí hello rozhraní příkazového řádku Azure

Azure Container Registry je spravovaná služba registru kontejnerů Dockeru sloužící k ukládání privátních imagí kontejnerů Dockeru. Tato příručka podrobně popisuje vytváření spravované pomocí instance registru kontejner Azure hello rozhraní příkazového řádku Azure.

Spravované registry kontejnerů Azure jsou ve verzi Preview a nejsou dostupné ve všech oblastech.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Pokud zvolíte tooinstall a místně pomocí hello rozhraní příkazového řádku, tento rychlý start vyžaduje, že používáte verzi rozhraní příkazového řádku Azure hello verze 2.0.4 nebo novější. Spustit `az --version` toofind hello verze. Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte skupinu prostředků s hello [vytvořit skupinu az](/cli/azure/group#create) příkaz. Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure. 

Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *westcentralus* umístění.

```azurecli-interactive 
az group create --name myResourceGroup --location westcentralus
```

## <a name="create-a-container-registry"></a>Vytvoření registru kontejnerů

Vytvoření instance ACR pomocí hello [vytvořit az acr](/cli/azure/acr#create) příkaz.

> [!NOTE]
> Při vytváření registru zadejte globálně jedinečný název domény nejvyšší úrovně obsahující pouze písmena a číslice.

 Název registru Hello v příkladu hello je *myContainerRegistry1*, nahraďte vlastní jedinečný název.

```azurecli
az acr create --name myContainerRegistry1 --resource-group myResourceGroup --sku Managed_Standard
```

Při vytváření hello registru výstup hello je podobné toohello následující:

```azurecli
{
  "adminUserEnabled": false,
  "creationDate": "2017-06-29T04:50:28.607134+00:00",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/myContainerRegistry1",
  "location": "westcentralus",
  "loginServer": "mycontainerregistry1.azurecr.io",
  "name": "myContainerRegistry1",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "name": "Managed_Standard",
    "tier": "Managed"
  },
  "storageAccount": null,
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

## <a name="log-in-tooacr-instance"></a>Přihlaste se tooACR instance

Před vkládání a stahování kontejneru bitové kopie, musíte být přihlášení toohello ACR instance. toodo Ano, použít hello [az acr přihlášení](/cli/azure/acr#login) příkaz.

```azurecli-interactive
az acr login --name myAzureContainerRegistry1
```

příkaz Hello vrátí zprávu byla úspěšná přihlášení po dokončení.

## <a name="use-azure-container-registry"></a>Použití služby Azure Container Registry

### <a name="list-container-images"></a>Výpis imagí kontejnerů

Použití hello `az acr` příkazy rozhraní příkazového řádku tooquery hello Image a značky v úložišti.

> [!NOTE]
> Kontejner registru v současné době nepodporuje hello `docker search` tooquery příkaz pro bitové kopie a značky.

### <a name="list-repositories"></a>Výpis úložišť

Hello následující příklad uvádí hello úložiště v registru, ve formátu JSON (JavaScript Object Notation):

```azurecli
az acr repository list -n myContainerRegistry1 -o json
```

### <a name="list-tags"></a>Výpis značek

Hello následující příklad vypíše značky hello na hello **ukázky/nginx** úložiště, ve formátu JSON:

```azurecli
az acr repository show-tags -n myContainerRegistry1 --repository samples/nginx -o json
```

## <a name="next-steps"></a>Další kroky

V této úvodní jste vytvořili spravované instance registru kontejner Azure pomocí hello rozhraní příkazového řádku Azure.

> [!div class="nextstepaction"]
> [Push vaší první image pomocí příkazového řádku Dockeru hello](container-registry-get-started-docker-cli.md)
