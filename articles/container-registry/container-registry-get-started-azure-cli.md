---
title: "aaaCreate privátní registru kontejner Docker - rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Začínáme vytváření a správě privátního registrech kontejner Docker s hello 2.0 rozhraní příkazového řádku Azure"
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: cristyg
tags: 
keywords: 
ms.assetid: 29e20d75-bf39-4f7d-815f-a2e47209be7d
ms.service: container-registry
ms.devlang: azurecli
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/06/2017
ms.author: stevelas
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f0d876a70b71a5e1bd564fbc9198f693dfe8a347
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-private-docker-container-registry-using-hello-azure-cli-20"></a>Vytvořit privátní registru kontejner Docker pomocí hello 2.0 rozhraní příkazového řádku Azure
Používání příkazů v hello [Azure CLI 2.0](https://github.com/Azure/azure-cli) toocreate kontejneru registru a jeho nastavení z počítače systému Linux, Mac nebo Windows. Můžete také vytvořit a spravovat pomocí hello registrech kontejneru [portál Azure](container-registry-get-started-portal.md) nebo prostřednictvím kódu programu hello kontejneru registru [REST API](https://go.microsoft.com/fwlink/p/?linkid=834376).


* Pozadí a koncepty najdete v tématu [hello – přehled](container-registry-intro.md)
* Nápovědu k registru kontejneru rozhraní příkazového řádku (`az acr` příkazy), předejte hello `-h` parametr tooany příkaz.


## <a name="prerequisites"></a>Požadavky
* **Azure CLI 2.0**: tooinstall a začít pracovat s hello 2.0 rozhraní příkazového řádku najdete v tématu hello [pokyny k instalaci](/cli/azure/install-azure-cli). Přihlaste se spuštěním tooyour předplatného Azure `az login`. Další informace najdete v tématu [začít pracovat s hello 2.0 rozhraní příkazového řádku](/cli/azure/get-started-with-azure-cli).
* **Skupina prostředků:** Než vytvoříte registr kontejnerů, vytvořte [skupinu prostředků](../azure-resource-manager/resource-group-overview.md#resource-groups) nebo použijte existující skupinu prostředků. Zkontrolujte, zda je skupina prostředků hello v umístění, kde je hello registru kontejneru služby [k dispozici](https://azure.microsoft.com/regions/services/). hello toocreate skupinu prostředků pomocí rozhraní příkazového řádku 2.0, najdete v části [hello referenční informace o rozhraní příkazového řádku 2.0](/cli/azure/group).
* **Účet úložiště** (volitelné): vytvořte standardní Azure [účet úložiště](../storage/common/storage-introduction.md) tooback hello kontejneru registru hello stejné umístění. Pokud nezadáte účet úložiště při vytváření registru s `az acr create`, hello příkaz vytvoří za vás. účet úložiště pomocí toocreate hello 2.0 rozhraní příkazového řádku najdete v tématu [hello referenční informace o rozhraní příkazového řádku 2.0](/cli/azure/storage/account). Storage úrovně Premium se v tuto chvíli nepodporuje.
* **Instanční objekt** (volitelné): Když vytvoříte soubor registru s hello rozhraní příkazového řádku, ve výchozím nastavení není nastaven pro přístup. Podle potřeby můžete přiřadit existujícího registru hlavní tooa služby Azure Active Directory (nebo vytvořit a přiřadit nový), nebo povolte hello registru Správce uživatelský účet. Najdete v části hello později v tomto článku. Další informace o přístup k registru najdete v tématu [ověřit s hello kontejneru registru](container-registry-authentication.md).

## <a name="create-a-container-registry"></a>Vytvoření registru kontejnerů
Spustit hello `az acr create` příkaz toocreate registru kontejneru.

> [!TIP]
> Při vytváření registru zadejte globálně jedinečný název domény nejvyšší úrovně obsahující pouze písmena a číslice. Název registru Hello v příkladech hello je `myRegistry1`, ale nahraďte vlastní jedinečný název.
>
>

Dobrý den, následující příkaz používá hello minimální parametry toocreate kontejneru registru `myRegistry1` ve skupině prostředků hello `myResourceGroup`a pomocí hello *základní* sku:

```azurecli
az acr create --name myRegistry1 --resource-group myResourceGroup --sku Basic
```

* Parametr `--storage-account-name` je volitelný. Pokud není zadán, s názvem, který se skládá z název registru hello je vytvoření účtu úložiště a časovým razítkem v hello zadat skupinu prostředků.

Při vytváření hello registru výstup hello je podobné toohello následující:

```azurecli
{
  "adminUserEnabled": false,
  "creationDate": "2017-06-06T18:36:29.124842+00:00",
  "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/myResourceGroup/providers/Microsoft.ContainerRegistry
/registries/myRegistry1",
  "location": "southcentralus",
  "loginServer": "myregistry1.azurecr.io",
  "name": "myRegistry1",
  "provisioningState": "Succeeded",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "storageAccount": {
    "name": "myregistry123456789"
  },
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}

```


Všimněte si zejména těchto hodnot:

* `id`-Identifikátor hello registru v rámci vašeho předplatného, které budete potřebovat, pokud chcete, aby tooassign hlavní název služby.
* `loginServer`-hello plně kvalifikovaný název zadáte příliš[protokolu v registru toohello](container-registry-authentication.md). V tomto příkladu je název hello `myregistry1.exp.azurecr.io` (všechny malá písmena).

## <a name="assign-a-service-principal"></a>Přiřazení instančního objektu
Pomocí rozhraní příkazového řádku 2.0 příkazy tooassign registru hlavní tooa služby Azure Active Directory. Hello instančního objektu v těchto příkladech je přiřazena hello vlastníka role, ale můžete přiřadit [jiné role](../active-directory/role-based-access-control-configure.md) podle potřeby.

### <a name="create-a-service-principal-and-assign-access-toohello-registry"></a>Vytvořit objekt služby a přiřadit přístup toohello registru
V hello následující příkaz, je přiřazen nový instanční objekt vlastníka role přístup toohello registru identifikátor byla dokončena s hello `--scopes` parametr. Zadejte silné heslo s hello `--password` parametr.

```azurecli
az ad sp create-for-rbac --scopes /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/myresourcegroup/providers/Microsoft.ContainerRegistry/registries/myregistry1 --role Owner --password myPassword
```



### <a name="assign-an-existing-service-principal"></a>Přiřazení existujícího instančního objektu
Pokud již máte hlavní název služby a chcete tooassign jeho vlastníka role přístup toohello registru, spusťte příkaz podobný toohello následující ukázka. Předat ID hlavní aplikace hello služby pomocí hello `--assignee` parametr:

```azurecli
az role assignment create --scope /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/myresourcegroup/providers/Microsoft.ContainerRegistry/registries/myregistry1 --role Owner --assignee myAppId
```



## <a name="manage-admin-credentials"></a>Správa přihlašovacích údajů správce
Účet správce se automaticky vytvoří pro každý registr kontejnerů a ve výchozím nastavení je vypnutý. Následující příklady zobrazují Hello `az acr` toomanage hello pověření správce pro vaše kontejneru registru příkazy rozhraní příkazového řádku.

### <a name="obtain-admin-user-credentials"></a>Získání přihlašovacích údajů uživatele s právy pro správu
```azurecli
az acr credential show -n myRegistry1
```

### <a name="enable-admin-user-for-an-existing-registry"></a>Povolení uživatele s právy pro správu pro existující registr
```azurecli
az acr update -n myRegistry1 --admin-enabled true
```

### <a name="disable-admin-user-for-an-existing-registry"></a>Zakázání uživatele s právy pro správu pro existující registr
```azurecli
az acr update -n myRegistry1 --admin-enabled false
```

## <a name="list-images-and-tags"></a>Výpis obrázků a značek
Použití hello `az acr` příkazy rozhraní příkazového řádku tooquery hello Image a značky v úložišti.

> [!NOTE]
> Kontejner registru v současné době nepodporuje hello `docker search` tooquery příkaz pro bitové kopie a značky.


### <a name="list-repositories"></a>Výpis úložišť
Hello následující příklad uvádí hello úložiště v registru, ve formátu JSON (JavaScript Object Notation):

```azurecli
az acr repository list -n myRegistry1 -o json
```

### <a name="list-tags"></a>Výpis značek
Hello následující příklad vypíše značky hello na hello **ukázky/nginx** úložiště, ve formátu JSON:

```azurecli
az acr repository show-tags -n myRegistry1 --repository samples/nginx -o json
```

## <a name="next-steps"></a>Další kroky
* [Push vaší první image pomocí příkazového řádku Dockeru hello](container-registry-get-started-docker-cli.md)
