---
title: "kurz pro službu kontejneru aaaAzure – Příprava aplikace | Microsoft Docs"
description: "Kurz pro Azure Container Service – Příprava aplikace"
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
ms.date: 07/25/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: b537ecc9ff50358fb65b128bfe6eb894dd088cc4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-container-images-toobe-used-with-azure-container-service"></a>Vytvoření kontejneru toobe Image použít s Azure Container Service

V tomto kurzu, 7, první část je více kontejnerové aplikace připravené pro použití v Kubernetes. Dokončit krokům patří:  

> [!div class="checklist"]
> * Klonování zdroje aplikace z GitHubu  
> * Vytvoření kontejneru image ze zdroje aplikace hello
> * Testování aplikace hello v místním prostředí Docker

Po dokončení hello následující aplikace je dostupné ve vašem místním vývojovém prostředí.

![Obrázek clusteru Kubernetes v Azure](media/container-service-kubernetes-tutorials/azure-vote.png)

V následujících kurzech hello kontejneru image je nahrané tooan registru kontejner Azure a pak spustit v Azure hostovaná Kubernetes clusteru.

## <a name="before-you-begin"></a>Než začnete

V tomto kurzu se předpokládá základní znalost klíčových konceptů Dockeru, jako jsou kontejnery, image kontejnerů a základní příkazy Dockeru. V případě potřeby najdete základní informace o kontejnerech v článku [Get started with Docker]( https://docs.docker.com/get-started/) (Začínáme s Dockerem). 

toocomplete tohoto kurzu budete potřebovat Docker vývojovém prostředí. Docker nabízí balíčky pro snadnou konfiguraci Dockeru na jakémkoli systému [Mac](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) nebo [Linux](https://docs.docker.com/engine/installation/#supported-platforms).

## <a name="get-application-code"></a>Získání kódu aplikace

Ukázková aplikace Hello použili v tomto kurzu je základní hlasovací aplikaci. Hello aplikace se skládá z front-endové webové součásti a instanci Redis back-end. součást webové Hello je zabalené do bitové kopie vlastní kontejner. Hello Redis instance používá image beze změny z úložiště Docker Hub.  

Pomocí git toodownload kopii hello aplikace tooyour vývojové prostředí.

```bash
git clone https://github.com/Azure-Samples/azure-voting-app-redis.git
```

Uvnitř hello klonovaný adresář je zdrojovému kódu aplikace hello, předem vytvořené Docker compose soubor a soubor manifestu Kubernetes. Tyto soubory jsou použité toocreate prostředky v rámci kurzu sadu hello. 

## <a name="create-container-images"></a>Vytvořit kontejner bitové kopie

[Docker Compose](https://docs.docker.com/compose/) lze použít sestavení hello tooautomate mimo kontejner bitové kopie a hello nasazení aplikací s více kontejneru.

Spustit hello docker-compose.yml souboru toocreate hello kontejneru image, stažení hello Redis bitové kopie a spusťte aplikaci hello.

```bash
docker-compose -f ./azure-voting-app-redis/docker-compose.yml up -d
```

Po dokončení použít hello [imagí dockeru](https://docs.docker.com/engine/reference/commandline/images/) příkaz toosee hello vytvoření bitové kopie.

```bash
docker images
```

Všimněte si, že tři bitové kopie byly staženy nebo vytvořeny. Hello *azure hlas front* image obsahuje aplikace hello. Je odvozený z hello *nginx flask* bitové kopie. Obrázek Redis Hello byl stažen z úložiště Docker Hub.

```bash
REPOSITORY                   TAG        IMAGE ID            CREATED             SIZE
azure-vote-front             latest     9cc914e25834        40 seconds ago      694MB
redis                        latest     a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask   flask      788ca94b2313        9 months ago        694MB
```

Spustit hello [docker ps](https://docs.docker.com/engine/reference/commandline/ps/) hello toosee příkaz spuštěných kontejnerů.

```bash
docker ps
```

Výstup:

```bash
CONTAINER ID        IMAGE             COMMAND                  CREATED             STATUS              PORTS                           NAMES
82411933e8f9        azure-vote-front  "/usr/bin/supervisord"   57 seconds ago      Up 30 seconds       443/tcp, 0.0.0.0:8080->80/tcp   azure-vote-front
b68fed4b66b6        redis             "docker-entrypoint..."   57 seconds ago      Up 30 seconds       0.0.0.0:6379->6379/tcp          azure-vote-back
```

## <a name="test-application-locally"></a>Testovací aplikace místně

Procházejte hello toosee toohttp://localhost:8080 spuštění aplikace.

![Obrázek clusteru Kubernetes v Azure](media/container-service-kubernetes-tutorials/azure-vote.png)

## <a name="clean-up-resources"></a>Vyčištění prostředků

Teď, když ověřila funkčnost aplikace hello spuštěných kontejnerů můžete zastavit a odebrat. Neodstraňujte hello kontejneru Image. Hello *azure hlas front* bitové kopie je instance nahrané tooan Azure kontejneru registru v dalším kurzu hello.

Spusťte hello následující toostop hello spuštěných kontejnerů.

```bash
docker-compose -f ./azure-voting-app-redis/docker-compose.yml stop
```

Odstranění kontejnerů hello zastavena s hello následující příkaz.

```bash
docker-compose -f ./azure-voting-app-redis/docker-compose.yml rm
```

Při dokončení máte kontejneru bitovou kopii, která obsahuje aplikaci Azure hlas hello.

## <a name="next-steps"></a>Další kroky

V tomto kurzu se testoval aplikace a bitové kopie kontejneru pro aplikace hello. byly dokončeny Hello následující kroky:

> [!div class="checklist"]
> * Klonování zdroj aplikace hello z Githubu  
> * Vytvořené bitové kopie kontejneru z zdroj aplikace
> * Otestované hello aplikace v místním prostředí Docker

Posunutí další kurz toolearn toohello o ukládání bitových kopií kontejneru v registru kontejneru služby Azure.

> [!div class="nextstepaction"]
> [Push bitové kopie tooAzure registru kontejneru](./container-service-tutorial-kubernetes-prepare-acr.md)
