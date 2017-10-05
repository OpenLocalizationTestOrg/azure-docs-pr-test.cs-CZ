---
title: "Azure CLI skriptu ukázkové – vytvoření aplikace pro funkce v rámci plánu služby App Service | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – vytvoření aplikace pro funkce v rámci plánu služby App Service"
services: functions
documentationcenter: functions
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 0e221db6-ee2d-4e16-9bf6-a456cd05b6e7
ms.service: functions
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 04/11/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 40c3fa6fa6c07d59e4bf55531e116ba50aa92b91
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-function-app-in-an-app-service-plan"></a>Vytvoření aplikace pro funkce v rámci plánu služby App Service

Tento ukázkový skript vytvoří aplikace funkce Azure, což je kontejner pro funkcí. Funkce aplikace je vytvořený pomocí vyhrazené plán služby App Service, což znamená, že vaše prostředky serveru jsou vždy zapnuty.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Ukázkový skript

Tento skript vytvoří aplikace funkce Azure pomocí vyhrazená [plán služby App Service](../functions-scale.md#app-service-plan).

[!code-azurecli-interactive[hlavní](../../../cli_scripts/azure-functions/create-function-app-app-service-plan/create-function-app-app-service-plan.sh "vytvoření funkce Azure na plán služby App Service")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Vysvětlení skriptu

Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz. Tento skript používá následující příkazy:

| Příkaz | Poznámky |
|---|---|
| [Vytvoření skupiny az](https://docs.microsoft.com/cli/azure/group#create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Vytvořit účet úložiště az](https://docs.microsoft.com/cli/azure/storage/account#create) | Vytvoří účet úložiště Azure. |
| [Vytvořit plán aplikační služby az](https://docs.microsoft.com/cli/azure/appserviceplan#create) | Vytvoří plán služby App Service. |
| [Vytvoření az functionapp](https://docs.microsoft.com/cli/azure/functionapp#delete) | Vytvoří aplikaci Azure funkce. |

## <a name="next-steps"></a>Další kroky

Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Další ukázky skriptu rozhraní příkazového řádku funkce Azure lze nalézt v [dokumentace Azure Functions](../functions-cli-samples.md).
