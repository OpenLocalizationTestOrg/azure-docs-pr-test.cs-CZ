---
title: "aaaCreate vaše první kontejner instancí kontejnerů Azure | Dokumentace Azure"
description: "Nasazení služby Azure Container Instances a zahájení práce"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: b57c52714933bd3b28c44d33f9af7cd1f23fb951
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-container-in-azure-container-instances"></a>Vytvoření prvního kontejneru ve službě Azure Container Instances

Azure instancí kontejnerů umožňuje snadno toocreate a Správa kontejnerů v Azure. V této úvodní vytvořit kontejner ve službě Azure a zveřejnění toohello internet s veřejnou IP adresu. K dokončení této operace stačí jediný příkaz. Během několika sekund uvidíte ve svém prohlížeči toto:

![Aplikace nasazená pomocí služby Azure Container Instances zobrazená v prohlížeči][aci-app-browser]

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Pokud zvolíte tooinstall a místně pomocí hello rozhraní příkazového řádku, tento rychlý start vyžaduje, že používáte verzi rozhraní příkazového řádku Azure hello 2.0.12 nebo novější. Spustit `az --version` toofind hello verze. Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Služba Azure Container Instances je prostředek Azure a musí být umístěná ve skupině prostředků Azure, což je logická kolekce, ve které se nasazují a spravují prostředky Azure.

Vytvořte skupinu prostředků s hello [vytvořit skupinu az](/cli/azure/group#create) příkaz. 

Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *eastus* umístění.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container"></a>Vytvoření kontejneru

Kontejner můžete vytvořit zadáním názvu, image Dockeru a skupiny prostředků Azure. Volitelně můžete vystavit hello kontejneru toohello internet s veřejnou IP adresu. V tomto případě použijeme kontejner, který je hostitelem velmi jednoduché webové aplikace napsané v [Node.js](http://nodejs.org).

```azurecli-interactive
az container create --name mycontainer --image microsoft/aci-helloworld --resource-group myResourceGroup --ip-address public 
```

Během pár sekund měli byste obdržet žádost o tooyour odpovědi. Na začátku hello kontejner bude v **vytváření** stavu, ale by se měl spustit během několika sekund. Můžete zkontrolovat stav hello pomocí hello `show` příkaz:

```azurecli-interactive
az container show --name mycontainer --resource-group myResourceGroup
```

V dolní části hello hello výstupu zobrazí se stav zřizování hello kontejner a jeho IP adresu:

```json
...
"ipAddress": {
      "ip": "13.88.8.148",
      "ports": [
        {
          "port": 80,
          "protocol": "TCP"
        }
      ]
    },
    "osType": "Linux",
    "provisioningState": "Succeeded"
...
```

Jakmile hello kontejneru přesune toohello **úspěšné** stavu, můžete dosáhnout v prohlížeči hello pomocí hello IP adresa zadaná. 

![Aplikace nasazená pomocí služby Azure Container Instances zobrazená v prohlížeči][aci-app-browser]

## <a name="pull-hello-container-logs"></a>Hello kontejneru protokoly pro vyžádání obsahu

Může vyžádat hello protokoly pro kontejner hello jste vytvořili pomocí hello `logs` příkaz:

```azurecli-interactive
az container logs --name mycontainer --resource-group myResourceGroup
```

Výstup:

```bash
listening on port 80
::ffff:10.240.255.105 - - [21/Jul/2017:00:01:46 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
::ffff:10.240.255.105 - - [21/Jul/2017:00:01:46 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://104.210.39.122/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
```

## <a name="delete-hello-container"></a>Odstranit kontejner hello

Až skončíte s hello kontejneru, můžete odebrat pomocí hello `delete` příkaz:

```azurecli-interactive
az container delete --name mycontainer --resource-group myResourceGroup
```

## <a name="next-steps"></a>Další kroky

Všechny hello kódu pro kontejner hello použitý v této úvodní je k dispozici [na Githubu][app-github-repo], společně s jeho soubor Docker. Pokud chcete tootry vytváření sami a jeho nasazení tooAzure kontejner instancí pomocí hello registru kontejner Azure, pokračujte v kurzu toohello instancí kontejnerů Azure.

> [!div class="nextstepaction"]
> [Kurzy služby Azure Container Instances](./container-instances-tutorial-prepare-app.md)


<!-- LINKS -->
[app-github-repo]: https://github.com/Azure-Samples/aci-helloworld.git

<!-- IMAGES -->
[aci-app-browser]: ./media/container-instances-quickstart/aci-app-browser.png