---
title: "Ukázka skriptu Azure CLI - škálování webovou aplikaci ručně pomocí Azure CLI 2.0 | Microsoft Docs"
description: "Ukázka skriptu Azure CLI - škálování webovou aplikaci ručně pomocí Azure CLI 2.0"
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 251d9074-8fff-4121-ad16-9eca9556ac96
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 12/11/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 961805842e9beca0ee2e34abcd0c1355f502a46b
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/12/2017
---
# <a name="scale-a-web-app-manually"></a>Ruční škálování webové aplikace

Tento ukázkový skript vytvoří skupinu prostředků, plán služby App Service a webové aplikace. Škáluje pak plán služby App Service z jediné instance na více instancí.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud si zvolíte instalaci a použití rozhraní příkazového řádku místně, je třeba Azure CLI verze 2.0 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/scale-manual/scale-manual.sh "Manual Scale")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy k vytvoření skupiny prostředků, webové aplikace a všechny související prostředky. Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| Příkaz | Poznámky |
|---|---|
| [`az group create`](/cli/azure/group?view=azure-cli-latest#az_group_create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [`az appservice plan create`](/cli/azure/appservice/plan?view=azure-cli-latest#az_appservice_plan_create) | Vytvoří plán služby App Service. |
| [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create) | Vytvoří webové aplikace Azure. |
| [`az appservice plan update`](/cli/azure/appservice/plan?view=azure-cli-latest#az_appservice_plan_update) | Aktualizace vlastnosti plánu služby App Service. |

## <a name="next-steps"></a>Další kroky

Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Další ukázky skript aplikace služby rozhraní příkazového řádku najdete v [dokumentaci služby Azure App Service](../app-service-cli-samples.md).
