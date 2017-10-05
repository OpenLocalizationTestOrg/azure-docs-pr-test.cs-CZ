---
title: "Kurz pro Azure Container Service – Příprava aplikace | Microsoft Docs"
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
ms.openlocfilehash: f02ee61ef1cd3b3dfaa051cfabe52866e3e7e838
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-container-images-to-be-used-with-azure-container-service"></a>Vytvoření bitové kopie kontejneru, který se má použít s Azure Container Service

V tomto kurzu, 7, první část je více kontejnerové aplikace připravené pro použití v Kubernetes. Dokončit krokům patří:  

> [!div class="checklist"]
> * Klonování zdroje aplikace z GitHubu  
> * Vytvoření kontejneru image ze zdroje aplikace
> * Testování aplikace v místním prostředí Docker

Po dokončení následující aplikace je dostupné ve vašem místním vývojovém prostředí.

![Obrázek clusteru Kubernetes v Azure](media/container-service-kubernetes-tutorials/azure-vote.png)

V následujících kurzech bitovou kopii kontejneru se nahraje registru kontejneru služby Azure a pak spustit v Azure hostovaná Kubernetes clusteru.

## <a name="before-you-begin"></a>Než začnete

V tomto kurzu se předpokládá základní znalost klíčových konceptů Dockeru, jako jsou kontejnery, image kontejnerů a základní příkazy Dockeru. V případě potřeby najdete základní informace o kontejnerech v článku [Get started with Docker]( https://docs.docker.com/get-started/) (Začínáme s Dockerem). 

K dokončení tohoto kurzu potřebujete vývojové prostředí pro Docker. Docker nabízí balíčky pro snadnou konfiguraci Dockeru na jakémkoli systému [Mac](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) nebo [Linux](https://docs.docker.com/engine/installation/#supported-platforms).

## <a name="get-application-code"></a>Získání kódu aplikace

Ukázková aplikace používá v tomto kurzu je základní hlasovací aplikaci. Aplikace se skládá z front-endové webové součásti a instanci Redis back-end. Součást webové je zabalené do bitové kopie vlastní kontejner. Redis instance používá image beze změny z úložiště Docker Hub.  

Pomocí git stáhnout kopii aplikace na svoje vývojové prostředí.

```bash
git clone https://github.com/Azure-Samples/azure-voting-app-redis.git
```

V adresáři klonovaný je zdrojový kód aplikace, předem vytvořené Docker compose soubor a soubor manifestu Kubernetes. Tyto soubory se používají k vytvoření prostředky v celé sadě kurzu. 

## <a name="create-container-images"></a>Vytvořit kontejner bitové kopie

[Docker Compose](https://docs.docker.com/compose/) můžete použít k automatizaci sestavení mimo kontejner bitové kopie a nasazení aplikací s více kontejneru.

Spusťte soubor docker-compose.yml na vytvoření bitové kopie kontejneru, stáhněte bitovou kopii Redis a spusťte aplikaci.

```bash
docker-compose -f ./azure-voting-app-redis/docker-compose.yml up -d
```

Po dokončení použít [imagí dockeru](https://docs.docker.com/engine/reference/commandline/images/) příkazu zobrazte vytvořené bitové kopie.

```bash
docker images
```

Všimněte si, že tři bitové kopie byly staženy nebo vytvořeny. *Azure hlas front* image obsahuje aplikace. Byl odebrán z *nginx flask* bitové kopie. Bitovou kopii Redis byl stažen z úložiště Docker Hub.

```bash
REPOSITORY                   TAG        IMAGE ID            CREATED             SIZE
azure-vote-front             latest     9cc914e25834        40 seconds ago      694MB
redis                        latest     a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask   flask      788ca94b2313        9 months ago        694MB
```

Spustit [docker ps](https://docs.docker.com/engine/reference/commandline/ps/) příkazu zobrazte spuštěných kontejnerů.

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

Přejděte na adrese http://localhost: 8080 zobrazíte běžící aplikaci.

![Obrázek clusteru Kubernetes v Azure](media/container-service-kubernetes-tutorials/azure-vote.png)

## <a name="clean-up-resources"></a>Vyčištění prostředků

Teď, když funkce aplikace byl ověřen, může spuštěných kontejnerů zastavena a odebrat. Neodstraňujte kontejneru bitové kopie. *Azure hlas front* bitové kopie se nahraje instanci Azure Container registru v dalším kurzu.

Spusťte následující příkaz k zastavení spuštěných kontejnerů.

```bash
docker-compose -f ./azure-voting-app-redis/docker-compose.yml stop
```

Odstranění zastaven kontejnerů pomocí následujícího příkazu.

```bash
docker-compose -f ./azure-voting-app-redis/docker-compose.yml rm
```

Při dokončení máte kontejneru bitovou kopii, která obsahuje aplikaci Azure hlas.

## <a name="next-steps"></a>Další kroky

V tomto kurzu aplikace byla testována a vytvořit kontejner bitových kopií pro aplikaci. Dokončili jste následující kroky:

> [!div class="checklist"]
> * Klonování zdroje aplikace z GitHubu  
> * Vytvořené bitové kopie kontejneru z zdroj aplikace
> * Testování aplikace v místním prostředí Docker

Přejděte k dalšímu kurzu, ve kterém se seznámíte s ukládáním imagí kontejnerů ve službě Azure Container Registry.

> [!div class="nextstepaction"]
> [Nahrávání imagí do služby Azure Container Registry](./container-service-tutorial-kubernetes-prepare-acr.md)