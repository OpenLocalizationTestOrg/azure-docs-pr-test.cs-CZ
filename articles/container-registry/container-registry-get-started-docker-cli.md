---
title: aaaPush Docker image tooprivate Azure registru | Microsoft Docs
description: "Push a načítat Docker registru privátní kontejneru tooa bitové kopie v Azure pomocí příkazového řádku Dockeru hello"
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: cristyg
tags: 
keywords: 
ms.assetid: 64fbe43f-fdde-4c17-a39a-d04f2d6d90a1
ms.service: container-registry
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/24/2017
ms.author: stevelas
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a81a6f4bfcb23642a89ac7631348d40e2f4911a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="push-your-first-image-tooa-private-docker-container-registry-using-hello-docker-cli"></a>Push vaší první obrázek tooa privátní Docker kontejneru registru pomocí příkazového řádku Dockeru hello
Azure kontejneru registru uchovává a spravuje privátní [Docker](http://hub.docker.com) kontejner imagí, podobné toohello způsob [úložiště Docker Hub](https://hub.docker.com/) ukládá veřejné imagí Dockeru. Použít hello [rozhraní příkazového řádku Dockeru](https://docs.docker.com/engine/reference/commandline/cli/) (příkazového řádku Dockeru) pro [přihlášení](https://docs.docker.com/engine/reference/commandline/login/), [nabízené](https://docs.docker.com/engine/reference/commandline/push/), [vyžádání](https://docs.docker.com/engine/reference/commandline/pull/)a další operace na vaše kontejneru registru.

Pozadí a koncepty najdete v tématu [hello – přehled](container-registry-intro.md)



## <a name="prerequisites"></a>Požadavky
* **Registr kontejnerů Azure** – Vytvořte registr kontejnerů ve svém předplatném Azure. Například použít hello [portál Azure](container-registry-get-started-portal.md) nebo hello [Azure CLI 2.0](container-registry-get-started-azure-cli.md).
* **Rozhraní příkazového řádku dockeru** -tooset do místního počítače jako Docker hostitele a přístup hello příkazového řádku Dockeru příkazy, nainstalujte [modulu Docker](https://docs.docker.com/engine/installation/).

## <a name="log-in-tooa-registry"></a>Přihlaste se tooa registru
Spustit `docker login` toolog v registru kontejneru tooyour s vaší [registru pověření](container-registry-authentication.md).

Hello následující příklad úspěšně projde hello ID a hesla služby Azure Active Directory [instanční objekt](../active-directory/active-directory-application-objects.md). Například může přiřadili jste hlavní tooyour registru služby automation scénář.

```
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

> [!TIP]
> Ujistěte se, že toospecify hello registru plně kvalifikovaný název (všechny malá písmena). V tomto příkladu je to `myregistry.azurecr.io`.

## <a name="steps-toopull-and-push-an-image"></a>Kroky toopull a nabízenými bitovou kopii
Hello podle příkladu stahování hello Nginx bitovou kopii z hello veřejného úložiště Docker Hub registru značky pro vaši privátní kontejner Azure registru nabízených oznámení tooyour registru a pak ho znovu vrátí.

**1. Hello Docker oficiální image pro vyžádání obsahu pro Nginx**

Nejprve pro vyžádání obsahu hello veřejné Nginx image tooyour místního počítače.

```
docker pull nginx
```
**2. Spusťte kontejner nginx a sváže hello**

Hello následující příkaz spustí místní kontejner nginx a sváže hello interaktivně na portu 8080, což vám toosee výstup z Nginx. Odebere hello systémem kontejneru jednou byla zastavena.

```
docker run -it --rm -p 8080:80 nginx
```

Procházet příliš[adrese http://localhost: 8080](http://localhost:8080) tooview hello systémem kontejneru. Zobrazí obrazovky podobné toohello následující jeden.

![Server Nginx na místním počítači](./media/container-registry-get-started-docker-cli/nginx.png)

toostop hello spuštěné kontejneru, stiskněte [CTRL] + [C].

**3. Vytvoření aliasu hello bitové kopie v registru**

Hello následující příkaz vytvoří alias hello bitové kopie s registru tooyour plně kvalifikovanou cestu. Tento příklad určuje hello `samples` zbytečné soubory tooavoid oboru názvů v kořenovém hello hello registru.

```
docker tag nginx myregistry.azurecr.io/samples/nginx
```  

**4. Push hello image tooyour registru**

```
docker push myregistry.azurecr.io/samples/nginx
```

**5. Hello image pro vyžádání obsahu z registru**

```
docker pull myregistry.azurecr.io/samples/nginx
```

**6. Spusťte kontejner nginx a sváže hello z registru**

```
docker run -it --rm -p 8080:80 myregistry.azurecr.io/samples/nginx
```

Procházet příliš[adrese http://localhost: 8080](http://localhost:8080) tooview hello systémem kontejneru.

toostop hello spuštěné kontejneru, stiskněte [CTRL] + [C].

**7. (Volitelné) Odeberte hello bitovou kopii**

```
docker rmi myregistry.azurecr.io/samples/nginx
```

##<a name="concurrent-limits"></a>Omezení souběžnosti
V některých scénářích provádění volání současně může vést k chybám. Hello následující tabulka obsahuje hello omezení souběžných volání s operace "Push" a "Pro vyžádání obsahu" na kontejner Azure registru:

| Operace  | Omezení                                  |
| ---------- | -------------------------------------- |
| PULL       | Až too10 souběžných vrátí na registru |
| PUSH       | Až too5 souběžných nabízených oznámení na registru |

## <a name="next-steps"></a>Další kroky
Nyní když znáte hello základy, jste připravené toostart pomocí vašeho klíče registru! Například začít s nasazením Image tooan kontejneru [Azure Container Service](https://azure.microsoft.com/documentation/services/container-service/) clusteru.
