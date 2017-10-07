---
title: "kurz instancí kontejnerů aaaAzure – nasazení aplikace | Microsoft Docs"
description: "Kurz pro Azure instancí kontejnerů – nasazení aplikace"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/04/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: b9fb098d9491e1073f0be4b14a0b9b1a18f16095
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-container-tooazure-container-instances"></a>Nasazení tooAzure kontejner instancí kontejnerů

Toto je hello poslední kurzu třemi částmi. V předchozích částech [bitovou kopii kontejner byl vytvořen](container-instances-tutorial-prepare-app.md) a [nabídnutých tooan registru kontejner Azure](container-instances-tutorial-prepare-acr.md). Tato část dokončení kurzu hello nasazením hello kontejneru tooAzure instancí kontejnerů. Dokončit krokům patří:

> [!div class="checklist"]
> * Definování skupinu kontejneru pomocí šablony Azure Resource Manager
> * Nasazení pomocí rozhraní příkazového řádku Azure hello skupina kontejneru hello
> * Protokoly kontejner zobrazení

## <a name="deploy-hello-container-using-hello-azure-cli"></a>Nasazení pomocí rozhraní příkazového řádku Azure hello kontejneru hello

Hello rozhraní příkazového řádku Azure umožňuje nasazení kontejneru tooAzure kontejner instancí v jednom příkazu. Vzhledem k tomu, že je hello kontejneru image umístěná v hello privátní registru kontejner Azure, je nutné zahrnout požadované tooaccess pověření hello ho. V případě potřeby se můžete dotazovat je jak je uvedeno níže.

Kontejner registru přihlášení server (aktualizace nahraďte názvem registru):

```azurecli-interactive
az acr show --name <acrName> --query loginServer
```

Kontejner registru heslo:

```azurecli-interactive
az acr credential show --name <acrName> --query "passwords[0].value"
```

toodeploy kontejneru image z registru hello kontejneru a zdroj žádosti o 1 jádro procesoru a 1GB paměti, spusťte následující příkaz hello:

```azurecli-interactive
az container create --name aci-tutorial-app --image <acrLoginServer>/aci-tutorial-app:v1 --cpu 1 --memory 1 --registry-password <acrPassword> --ip-address public -g myResourceGroup
```

Během pár sekund zobrazí se první odpověď ze Správce prostředků Azure. Stav hello tooview hello nasazení, použijte:

```azurecli-interactive
az container show --name aci-tutorial-app --resource-group myResourceGroup --query state
```

Abychom mohli pokračovat, dokud hello stav se změní z spuštěním tohoto příkazu *čekající* příliš*systémem*. Potom jsme můžete pokračovat.

## <a name="view-hello-application-and-container-logs"></a>Zobrazit protokoly aplikací a kontejner hello

Po úspěšné nasazení hello, otevřete váš prohlížeč toohello IP adresu ukazuje výstup hello hello následující příkaz:

```bash
az container show --name aci-tutorial-app --resource-group myResourceGroup --query ipAddress.ip
```

```json
"13.88.176.27"
```

![Hello world aplikaci v prohlížeči hello][aci-app-browser]

Můžete také zobrazit výstup hello protokolu hello kontejneru:

```azurecli-interactive
az container logs --name aci-tutorial-app -g myResourceGroup
```

Výstup:

```bash
listening on port 80
::ffff:10.240.0.4 - - [21/Jul/2017:06:00:02 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
::ffff:10.240.0.4 - - [21/Jul/2017:06:00:02 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://13.88.176.27/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
```

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste dokončili proces hello nasazení vaší tooAzure kontejnery instancí kontejnerů. byly dokončeny Hello následující kroky:

> [!div class="checklist"]
> * Nasazení kontejneru hello pomocí registru kontejner Azure hello hello rozhraní příkazového řádku Azure
> * Zobrazení aplikace hello v prohlížeči hello
> * Zobrazení hello kontejneru protokoly

<!-- LINKS -->
[prepare-app]: ./container-instances-tutorial-prepare-app.md

<!-- IMAGES -->
[aci-app-browser]: ./media/container-instances-quickstart/aci-app-browser.png
