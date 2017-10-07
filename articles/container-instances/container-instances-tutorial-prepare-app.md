---
title: "aaaAzure instancí kontejnerů kurz – Příprava aplikace | Dokumentace Azure"
description: "Příprava aplikace pro nasazení tooAzure instancí kontejnerů"
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
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/01/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 406ba796e5fefb1527f2e894cc3f7bbd8f7a5fd1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-container-for-deployment-tooazure-container-instances"></a>Vytváření kontejneru pro nasazení tooAzure instancí kontejnerů

Azure Container Instances umožňuje nasazení kontejnerů Dockeru na infrastrukturu Azure bez zřizování virtuálních počítačů nebo využívání služby vyšší úrovně. V tomto kurzu vytvoříte jednoduchou webovou aplikaci v Node.js a zabalíte ji do kontejneru, který bude možné spustit pomocí služby Azure Container Instances. Společně probereme:

> [!div class="checklist"]
> * Klonování zdroje aplikace z GitHubu  
> * Vytváření imagí kontejnerů ze zdroje aplikace
> * Testování obrázků hello v místním prostředí Docker

V následujících kurzech nahrát váš obrázek tooan registru kontejner Azure a pak je nasadit tooAzure kontejner instancí.

## <a name="before-you-begin"></a>Než začnete

V tomto kurzu se předpokládá základní znalost klíčových konceptů Dockeru, jako jsou kontejnery, image kontejnerů a základní příkazy Dockeru. V případě potřeby najdete základní informace o kontejnerech v článku [Get started with Docker]( https://docs.docker.com/get-started/) (Začínáme s Dockerem). 

toocomplete tohoto kurzu budete potřebovat Docker vývojovém prostředí. Docker nabízí balíčky pro snadnou konfiguraci Dockeru na jakémkoli systému [Mac](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) nebo [Linux](https://docs.docker.com/engine/installation/#supported-platforms).

## <a name="get-application-code"></a>Získání kódu aplikace

Ukázka Hello v tomto kurzu zahrnuje jednoduchou webovou aplikaci součástí [Node.js](http://nodejs.org). aplikace Hello slouží statické stránky HTML a vypadá takto:

![Ukázková aplikace zobrazená v prohlížeči][aci-tutorial-app]

Ukázka použití git toodownload hello:

```bash
git clone https://github.com/Azure-Samples/aci-helloworld.git
```

## <a name="build-hello-container-image"></a>Sestavení hello kontejneru bitové kopie

Hello zadaná v úložišti hello ukázkový soubor Docker ukazuje, jak je integrovaná hello kontejneru. Spuštění z [oficiální Node.js image] [ dockerhub-nodeimage] na základě [Alpine Linux](https://alpinelinux.org/), malé rozdělení, na které se nehodí toouse s kontejnery. Ji pak zkopíruje soubory aplikace hello do kontejneru hello, nainstaluje závislosti s využitím hello Node Package Manager a nakonec spustí aplikaci hello.

```
FROM node:8.2.0-alpine
RUN mkdir -p /usr/src/app
COPY ./app/* /usr/src/app/
WORKDIR /usr/src/app
RUN npm install
CMD node /usr/src/app/index.js
```

Použití hello `docker build` toocreate hello kontejneru obrázek příkazu, označování jej jako *aci – kurz aplikace*:

```bash
docker build ./aci-helloworld -t aci-tutorial-app
```

Použití hello `docker images` toosee hello vytvořené bitové kopie:

```bash
docker images
```

Výstup:

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED              SIZE
aci-tutorial-app             latest              5c745774dfa9        39 seconds ago       68.1 MB
```

## <a name="run-hello-container-locally"></a>Spusťte kontejner hello místně

Před nasazením hello kontejneru tooAzure kontejner instancí se jej spusťte místně tooconfirm, zda funguje. Hello `-d` přepínač umožňuje hello kontejner spuštěny hello pozadí, zatímco `-p` vám umožní toomap na vaše výpočetní tooport 80 kontejneru hello libovolný port.

```bash
docker run -d -p 8080:80 aci-tutorial-app
```

Otevřete hello tooconfirm toohttp://localhost:8080 prohlížeče, který hello kontejneru je spuštěna.

![Spuštěné aplikaci hello místně v prohlížeči hello][aci-tutorial-app-local]

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste vytvořili kontejner bitovou kopii, která může být nasazený tooAzure kontejner instancí. byly dokončeny Hello následující kroky:

> [!div class="checklist"]
> * Klonování zdroj aplikace hello z Githubu  
> * Vytváření imagí kontejnerů ze zdroje aplikace
> * Testování hello kontejneru místně

Posunutí další kurz toolearn toohello o ukládání bitových kopií kontejneru v registru kontejneru služby Azure.

> [!div class="nextstepaction"]
> [Push bitové kopie tooAzure registru kontejneru](./container-instances-tutorial-prepare-acr.md)

<!-- LINKS -->
[dockerhub-nodeimage]: https://hub.docker.com/r/library/node/tags/8.2.0-alpine/

<!--- IMAGES --->
[aci-tutorial-app]:./media/container-instances-quickstart/aci-app-browser.png
[aci-tutorial-app-local]: ./media/container-instances-tutorial-prepare-app/aci-app-browser-local.png