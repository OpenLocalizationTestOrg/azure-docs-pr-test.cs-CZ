---
title: "aaaManage Azure Swarm clusteru s rozhraním API Docker | Microsoft Docs"
description: "Nasazení clusteru Docker Swarm kontejnery tooa v Azure Container Service"
services: container-service
documentationcenter: 
author: rgardler
manager: madhana
editor: 
tags: acs, azure-container-service
keywords: "Docker, kontejnery, mikroslužby, Mesos, Azure"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/13/2016
ms.author: rogardle
ms.custom: mvc
ms.openlocfilehash: bb9b07c82a7b48caeb2e351455797cbf2a6e7480
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="container-management-with-docker-swarm"></a>Správa kontejnerů pomocí nástroje Docker Swarm
Docker Swarm poskytuje prostředí pro nasazování kontejnerizovaných úloh v celé sadě hostitelů Docker uspořádaných do fondu. Docker Swarm používá hello nativní Docker API. Hello workflow správy kontejnerů v nástroji Docker Swarm je téměř identický toowhat, které má být na hostitele s jedním kontejnerem. Tento dokument poskytuje jednoduché příklady, jak nasadit kontejnerizované úlohy do instance Docker Swarm v Azure Container Service. Podrobnější dokumentaci k nástroji Docker Swarm najdete v tématu [Docker Swarm na Docker.com](https://docs.docker.com/swarm/).

[!INCLUDE [container-service-swarm-mode-note](../../../includes/container-service-swarm-mode-note.md)]

Cvičení toohello požadavky v tomto dokumentu:

[Vytvoření clusteru Swarm v Azure Container Service](container-service-deployment.md)

[Propojení s clusterem Swarm hello v Azure Container Service](../container-service-connect.md)

## <a name="deploy-a-new-container"></a>Nasazení nového kontejneru
toocreate nový kontejner v hello Docker Swarm, použijte hello `docker run` příkazu (zajistíte, že máte otevřený SSH tunel toohello hlavních serverů podle výš uvedené požadavky hello). Tento příklad vytvoří kontejner z hello `yeasy/simple-web` bitové kopie:

```bash
user@ubuntu:~$ docker run -d -p 80:80 yeasy/simple-web

4298d397b9ab6f37e2d1978ef3c8c1537c938e98a8bf096ff00def2eab04bf72
```

Po vytvoření kontejneru hello použít `docker ps` tooreturn informace o kontejneru hello. Můžete si zde všimněte, že je uvedený tohoto agenta Swarm hello, který je hostitelem kontejneru hello:

```bash
user@ubuntu:~$ docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                 NAMES
4298d397b9ab        yeasy/simple-web    "/bin/sh -c 'python i"   31 seconds ago      Up 9 seconds        10.0.0.5:80->80/tcp   swarm-agent-34A73819-1/happy_allen
```  

Nyní máte přístup hello aplikace, která běží v tomto kontejneru prostřednictvím hello veřejný název DNS služby Vyrovnávání zatížení agenta Swarm hello. Tyto informace můžete najít v hello portálu Azure:  

![Výsledky skutečných návštěv](./media/container-service-docker-swarm/real-visit.jpg)  

Ve výchozím nastavení má hello nástroj pro vyrovnávání zatížení porty 80, 8080 a 443 otevřete. Pokud chcete, aby tooconnect na jiný port musíte tooopen tento port na hello nástroj pro vyrovnávání zatížení Azure pro hello fondu agenta.

## <a name="deploy-multiple-containers"></a>Nasazení několika kontejnerů
Jak je spuštěno více kontejnerů, spuštěním 'docker spustit' vícekrát, můžete použít hello `docker ps` toosee příkaz, který hostuje hello kontejnery běží. V příkladu hello níže jsou tři kontejnery rovnoměrně rozloženy hello tři agenty Swarm:  

```bash
user@ubuntu:~$ docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                 NAMES
11be062ff602        yeasy/simple-web    "/bin/sh -c 'python i"   11 seconds ago      Up 10 seconds       10.0.0.6:83->80/tcp   swarm-agent-34A73819-2/clever_banach
1ff421554c50        yeasy/simple-web    "/bin/sh -c 'python i"   49 seconds ago      Up 48 seconds       10.0.0.4:82->80/tcp   swarm-agent-34A73819-0/stupefied_ride
4298d397b9ab        yeasy/simple-web    "/bin/sh -c 'python i"   2 minutes ago       Up 2 minutes        10.0.0.5:80->80/tcp   swarm-agent-34A73819-1/happy_allen
```  

## <a name="deploy-containers-by-using-docker-compose"></a>Nasazení kontejnerů pomocí Docker Compose
Můžete použít Docker Compose tooautomate hello nasazení a konfiguraci několika kontejnerů. toodo Ano, zkontrolujte, zda je vytvořen tunel Secure Shell (SSH) a byla nastavena proměnná DOCKER_HOST této hello (viz hello požadavky výše).

Vytvořte v lokálním systému soubor docker-compose.yml. toodo, použijte ji [ukázka](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/docker-compose.yml).

```bash
web:
  image: adtd/web:0.1
  ports:
    - "80:80"
  links:
    - rest:rest-demo-azure.marathon.mesos
rest:
  image: adtd/rest:0.1
  ports:
    - "8080:8080"

```

Spusťte `docker-compose up -d` nasazení kontejnerů toostart hello:

```bash
user@ubuntu:~/compose$ docker-compose up -d
Pulling rest (adtd/rest:0.1)...
swarm-agent-3B7093B8-0: Pulling adtd/rest:0.1... : downloaded
swarm-agent-3B7093B8-2: Pulling adtd/rest:0.1... : downloaded
swarm-agent-3B7093B8-3: Pulling adtd/rest:0.1... : downloaded
Creating compose_rest_1
Pulling web (adtd/web:0.1)...
swarm-agent-3B7093B8-3: Pulling adtd/web:0.1... : downloaded
swarm-agent-3B7093B8-0: Pulling adtd/web:0.1... : downloaded
swarm-agent-3B7093B8-2: Pulling adtd/web:0.1... : downloaded
Creating compose_web_1
```

Nakonec se vrátí hello seznam spuštěných kontejnerů. Tento seznam obsahuje hello kontejnery, které byly nasazeny pomocí Docker Compose:

```bash
user@ubuntu:~/compose$ docker ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS                     NAMES
caf185d221b7        adtd/web:0.1        "apache2-foreground"   2 minutes ago       Up About a minute   10.0.0.4:80->80/tcp       swarm-agent-3B7093B8-0/compose_web_1
040efc0ea937        adtd/rest:0.1       "catalina.sh run"      3 minutes ago       Up 2 minutes        10.0.0.4:8080->8080/tcp   swarm-agent-3B7093B8-0/compose_rest_1
```

Samozřejmě můžete použít `docker-compose ps` tooexamine pouze hello kontejnery, které jsou definované v vaší `compose.yml` souboru.

## <a name="next-steps"></a>Další kroky
[Další informace o Docker Swarmu](https://docs.docker.com/swarm/)

