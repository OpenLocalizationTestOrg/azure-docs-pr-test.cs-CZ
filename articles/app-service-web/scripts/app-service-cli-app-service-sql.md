---
title: "Azure CLI skriptu ukázkové – webovou aplikaci připojit k databázi SQL | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – webovou aplikaci připojit k databázi SQL"
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 7c2efdd0-f553-4038-a77a-e953021b3f77
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 06/19/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: ec5e22bfacc12a89f1fb5882487df4829369777c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="connect-a-web-app-to-a-sql-database"></a>Webovou aplikaci připojit k databázi SQL

V tomto scénáři se dozvíte, jak vytvářet Azure SQL database a webové aplikace Azure. Potom propojíte databáze SQL pro webovou aplikaci pomocí nastavení aplikace.


[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli-interactive[hlavní](../../../cli_scripts/app-service/connect-to-sql/connect-to-sql.sh?highlight=9-10 "databáze SQL")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy k vytvoření skupiny prostředků, webové aplikace, databáze SQL a všechny související prostředky. Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| Příkaz | Poznámky |
|---|---|
| [Vytvoření skupiny az](https://docs.microsoft.com/cli/azure/group#create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Vytvořit plán aplikační služby az](https://docs.microsoft.com/cli/azure/appservice/plan#create) | Vytvoří plán služby App Service. Toto je jako serverové farmy pro Azure webové aplikace. |
| [Vytvoření az webapp](https://docs.microsoft.com/cli/azure/webapp#create) | Vytvoří webové aplikace Azure. |
| [Vytvoření az sql serveru](https://docs.microsoft.com/cli/azure/sql/server#create) | Vytvoří databázi serveru SQL.  |
| [Vytvoření az sql db](https://docs.microsoft.com/cli/azure/sql/db#create) | Vytvoří novou databázi s databázovým serverem SQL. |
| [AZ webapp konfigurace appsettings sady](https://docs.microsoft.com/cli/azure/webapp/config/appsettings#set) | Vytvoří nebo aktualizuje nastavení aplikace pro webové aplikace Azure. Nastavení aplikace jsou zveřejněné jako proměnné prostředí pro vaši aplikaci. |

## <a name="next-steps"></a>Další kroky

Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Další ukázky skript aplikace služby rozhraní příkazového řádku najdete v [dokumentaci služby Azure App Service](../app-service-cli-samples.md).
