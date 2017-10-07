---
title: "aaaAzure ukázky rozhraní příkazového řádku - App Service | Microsoft Docs"
description: "Ukázky rozhraní příkazového řádku Azure - služby App Service"
services: app-service
documentationcenter: app-service
author: syntaxc4
manager: erikre
editor: ggailey777
tags: azure-service-management
ms.assetid: 53e6a15a-370a-48df-8618-c6737e26acec
ms.service: app-service
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: app-service
ms.date: 03/08/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: a943ccffb59c5d30a44cf1ce513fd2eac46101f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cli-samples"></a>Ukázky rozhraní příkazového řádku Azure

Hello následující tabulka obsahuje odkazy toobash skripty vyvíjené hello rozhraní příkazového řádku Azure.

| | |
|-|-|
|**Vytvoření aplikace**||
| [Vytvoření webové aplikace a nasazení kódu z Githubu](./scripts/app-service-cli-deploy-github.md?toc=%2fcli%2fazure%2ftoc.json)| Webové aplikace Azure vytvoří a nasadí kód z veřejného úložiště GitHub. |
| [Vytvoření webové aplikace s průběžným nasazováním z Githubu](./scripts/app-service-cli-continuous-deployment-github.md?toc=%2fcli%2fazure%2ftoc.json)| Vytvoří webové aplikace Azure s nepřetržitou publikování z úložiště Githubu, které vlastníte. |
| [Vytvoření webové aplikace a nasazení kódu z místního úložiště Git](./scripts/app-service-cli-deploy-local-git.md?toc=%2fcli%2fazure%2ftoc.json) | Vytvoří webové aplikace Azure a nakonfiguruje nabízené kód z místního úložiště Git. |
| [Vytvoření webové aplikace a nasazení kódu tooa pracovní prostředí](./scripts/app-service-cli-deploy-staging-environment.md?toc=%2fcli%2fazure%2ftoc.json) | Vytvoří se nasazovací slot pro přípravu změny kódu webové aplikace Azure. |
| [Vytvoření webové aplikace ASP.NET Core v kontejneru Docker](./scripts/app-service-cli-linux-docker-aspnetcore.md?toc=%2fcli%2fazure%2ftoc.json)| Vytvoří webové aplikace Azure v systému Linux a načte bitovou kopii Docker z úložiště Docker Hub. |
|**Konfigurace aplikace**||
| [Mapa vlastní domény tooa webové aplikace](./scripts/app-service-cli-configure-custom-domain.md?toc=%2fcli%2fazure%2ftoc.json)| Vytvoří webové aplikace Azure a mapuje tooit název vlastní doménu. |
| [Vytvoření vazby vlastní tooa webové aplikace certifikát SSL](./scripts/app-service-cli-configure-ssl-certificate.md?toc=%2fcli%2fazure%2ftoc.json)| Vytvoří webové aplikace Azure a vytvoří vazbu certifikátu SSL hello tooit název vlastní doménu. |
|**Škálování aplikace**||
| [Ruční škálování webové aplikace](./scripts/app-service-cli-scale-manual.md?toc=%2fcli%2fazure%2ftoc.json) | Vytvoří webové aplikace Azure a škáluje napříč 2 instance. |
| [Škálování webové aplikace po celém světě s využitím architektury s vysokou dostupností](./scripts/app-service-cli-scale-high-availability.md?toc=%2fcli%2fazure%2ftoc.json) | Vytvoří dvě službě Azure web apps ve dvou různých zeměpisných oblastech a jsou pak dostupné přes jeden koncový bod pomocí Azure Traffic Manager. |
|**Připojení aplikace tooresources**||
| [Připojení webové aplikaci tooa databáze SQL](./scripts/app-service-cli-app-service-sql.md?toc=%2fcli%2fazure%2ftoc.json)| Vytvoří webové aplikace Azure a SQL database a potom přidá hello databáze připojovací řetězec toohello nastavení aplikace. |
| [Připojení účtu úložiště tooa webové aplikace](./scripts/app-service-cli-app-service-storage.md?toc=%2fcli%2fazure%2ftoc.json)| Vytvoří webové aplikace Azure a účet úložiště a potom přidá hello úložiště připojovací řetězec toohello nastavení aplikace. |
| [Připojit mezipaměti redis tooa webové aplikace](./scripts/app-service-cli-app-service-redis.md?toc=%2fcli%2fazure%2ftoc.json) | Vytvoří webové aplikace Azure a mezipaměti redis a potom přidá hello redis připojení podrobnosti toohello nastavení aplikace.) |
| [Připojení webové aplikaci tooCosmos DB](./scripts/app-service-cli-app-service-documentdb.md?toc=%2fcli%2fazure%2ftoc.json) | Vytvoří webové aplikace Azure a Cosmos DB a potom přidá hello Cosmos DB připojení podrobnosti toohello nastavení aplikace. |
|**Monitorování aplikace**||
| [Monitorování webové aplikace pomocí protokolů webového serveru](./scripts/app-service-cli-monitor.md?toc=%2fcli%2fazure%2ftoc.json) | Vytvoří webové aplikace Azure, povolí protokolování pro něj a stáhne hello protokoly tooyour místního počítače. |
| | |