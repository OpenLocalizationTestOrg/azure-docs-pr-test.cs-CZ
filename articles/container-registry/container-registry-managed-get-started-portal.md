---
title: "aaaCreate privátní registru Docker - portálu Azure | Microsoft Docs"
description: "Začínáme vytváření a správě privátního registrech kontejner Docker s hello portálu Azure"
services: container-registry
documentationcenter: 
author: neilpeterson
manager: timlt
editor: na
tags: 
keywords: 
ms.assetid: 53a3b3cb-ab4b-4560-bc00-366e2759f1a1
ms.service: container-registry
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/11/2017
ms.author: nepeters
ms.custom: na
ms.openlocfilehash: cf3ce0dcf3036d0e9cd1eaf01721deccb00248d2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-managed-container-registry-using-hello-azure-portal"></a>Vytvoření spravované kontejneru registru pomocí hello portálu Azure

Azure Container Registry je spravovaná služba registru kontejnerů Dockeru sloužící k ukládání privátních imagí kontejnerů Dockeru. Tato příručka podrobně popisuje vytváření spravované pomocí instance registru kontejner Azure hello portálu Azure.

Spravované registry kontejnerů Azure jsou ve verzi Preview a nejsou dostupné ve všech oblastech.

## <a name="log-in-tooazure"></a>Přihlaste se tooAzure

Přihlaste se toohello portál Azure na http://portal.azure.com.

## <a name="create-a-container-registry"></a>Vytvoření registru kontejnerů

1. Klikněte na tlačítko hello **nový** nalezeno tlačítko na hello levém horním rohu hello portálu Azure.

2. Hledání hello marketplace pro **kontejner Azure registru** a vyberte ho.

3. Klikněte na tlačítko **vytvořit** které se otevře okno pro vytvoření ACR hello.

    ![Nastavení registru kontejnerů](./media/container-registry-get-started-portal/managed-container-registry-settings.png)

4. V hello **registru kontejner Azure** okno, zadejte následující informace hello. Až budete hotovi, klikněte na **Vytvořit**.

    a. **Název registru:** Globálně jedinečný název domény nejvyšší úrovně konkrétního registru. V tomto příkladu je název registru hello *myAzureContainerRegistry1*, ale nahraďte vlastní jedinečný název. Hello název může obsahovat jenom písmena a číslice.

    b. **Skupina prostředků**: Vyberte existující [skupiny prostředků](../azure-resource-manager/resource-group-overview.md#resource-groups) nebo název typu hello nové.

    c. **Umístění**: Vyberte datové centrum Azure umístění, kde je služba hello [k dispozici](https://azure.microsoft.com/regions/services/), jako například **jihu USA**.

    d. **Uživatel s oprávněními správce**: Pokud chcete, povolení registru hello tooaccess uživatele správce. Toto nastavení po vytvoření hello registru můžete změnit.

    e. **Použití spravovaných registru**: klepněte na tlačítko Ano toohave ACR automaticky spravovat úložiště hello registru, pomocí webhooků a použít ověřování AAD.

    f. **Cenová úroveň:** Vyberte cenovou úroveň; další informace najdete tady v cenách služby ACR.

## <a name="log-in-tooacr-instance"></a>Přihlaste se tooACR instance

Před vkládání a stahování kontejneru bitové kopie, musíte být přihlášení toohello ACR instance. 

toodo Ano, použít hello 2.0 rozhraní příkazového řádku Azure. Nejprve v případě potřeby, přihlaste se k Azure pomocí hello [az přihlášení](/cli/azure/#login) příkaz. 

```azurecli
az login
```

Pak pomocí hello [az acr přihlášení](/cli/azure/acr#login) příkaz toolog v toohello registru kontejner Azure.

```azurecli-interactive
az acr login --name myAzureContainerRegistry1
```

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

V této úvodní jste vytvořili spravované instance registru kontejner Azure pomocí hello portálu Azure.

> [!div class="nextstepaction"]
> [Push vaší první image pomocí příkazového řádku Dockeru hello](container-registry-get-started-docker-cli.md)
