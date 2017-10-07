---
title: "aaaAzure ukázky PowerShell - App Service | Microsoft Docs"
description: "Ukázky Azure PowerShell - služby App Service"
services: app-service
documentationcenter: app-service
author: syntaxc4
manager: erikre
editor: ggailey777
tags: azure-service-management
ms.assetid: b48d1137-8c04-46e0-b430-101e07d7e470
ms.service: app-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: app-service
ms.date: 03/08/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: b7b4a030364f797195522c56fbae5b7f530d4d1b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-powershell-samples"></a>Ukázky Azure PowerShell

Hello následující tabulka obsahuje odkazy toobash skripty vyvíjené hello prostředí Azure PowerShell.

| | |
|-|-|
|**Vytvoření aplikace**||
| [Vytvoření webové aplikace s nasazením z Githubu](./scripts/app-service-powershell-deploy-github.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Vytvoří Azure webové aplikace, která vrátí kód z Githubu. |
| [Vytvoření webové aplikace s průběžným nasazováním z Githubu](./scripts/app-service-powershell-continuous-deployment-github.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Vytvoří Azure webové aplikace, které průběžně nasadí kód z Githubu. |
| [Vytvoření webové aplikace a nasazení kódu do serveru FTP](./scripts/app-service-powershell-deploy-ftp.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Vytvoří službě Azure web app a nahrávání souborů z místního adresáře pomocí protokolu FTP. |
| [Vytvoření webové aplikace a nasazení kódu z místního úložiště Git](./scripts/app-service-powershell-deploy-local-git.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Vytvoří webové aplikace Azure a nakonfiguruje nabízené kód z místního úložiště Git. |
| [Vytvoření webové aplikace a nasazení kódu tooa pracovní prostředí](./scripts/app-service-powershell-deploy-staging-environment.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Vytvoří se nasazovací slot pro přípravu změny kódu webové aplikace Azure. |
|**Konfigurace aplikace**||
| [Mapa vlastní domény tooa webové aplikace](./scripts/app-service-powershell-configure-custom-domain.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Vytvoří webové aplikace Azure a mapuje tooit název vlastní doménu. |
| [Vytvoření vazby vlastní tooa webové aplikace certifikát SSL](./scripts/app-service-powershell-configure-ssl-certificate.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Vytvoří webové aplikace Azure a vytvoří vazbu certifikátu SSL hello tooit název vlastní doménu. |
|**Škálování aplikace**||
| [Ruční škálování webové aplikace](./scripts/app-service-powershell-scale-manual.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Vytvoří webové aplikace Azure a škáluje napříč 2 instance. |
| [Škálování webové aplikace po celém světě s využitím architektury s vysokou dostupností](./scripts/app-service-powershell-scale-high-availability.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Vytvoří dvě službě Azure web apps ve dvou různých zeměpisných oblastech a jsou pak dostupné přes jeden koncový bod pomocí Azure Traffic Manager. |
|**Připojení aplikace tooresources**||
| [Připojení webové aplikaci tooa databáze SQL](./scripts/app-service-powershell-connect-to-sql.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Vytvoří webové aplikace Azure a SQL database a potom přidá hello databáze připojovací řetězec toohello nastavení aplikace. |
| [Připojení účtu úložiště tooa webové aplikace](./scripts/app-service-powershell-connect-to-storage.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Vytvoří webové aplikace Azure a účet úložiště a potom přidá hello úložiště připojovací řetězec toohello nastavení aplikace. |
|**Monitorování aplikace**||
| [Monitorování webové aplikace pomocí protokolů webového serveru](./scripts/app-service-powershell-monitor.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Vytvoří webové aplikace Azure, povolí protokolování pro něj a stáhne hello protokoly tooyour místního počítače. |
| | |
