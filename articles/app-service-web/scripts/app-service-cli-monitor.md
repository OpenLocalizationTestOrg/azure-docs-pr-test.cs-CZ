---
title: "Ukázka skriptu Azure CLI – monitorování webové aplikace s protokoly webového serveru | Microsoft Docs"
description: "Ukázka skriptu Azure CLI – monitorování webové aplikace s protokoly webového serveru"
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 0887656f-611c-4627-8247-b5cded7cef60
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 06/19/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: df4ca5b1270ada849e231ad9608a5b1d2edda8be
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-a-web-app-with-web-server-logs"></a>Monitorování webové aplikace s protokoly webového serveru

V tomto scénáři vytvoříte skupinu prostředků, plán služby app service, webové aplikace a konfigurace webové aplikace k povolení protokolů webového serveru. Pak si stáhnete souborů protokolu ke kontrole.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli-interactive[hlavní](../../../cli_scripts/app-service/monitor-with-logs/monitor-with-logs.sh "monitorování protokolů")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy k vytvoření skupiny prostředků, webové aplikace a všechny související prostředky. Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| Příkaz | Poznámky |
|---|---|
| [Vytvoření skupiny az](https://docs.microsoft.com/cli/azure/group#create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Vytvořit plán aplikační služby az](https://docs.microsoft.com/cli/azure/appservice/plan#create) | Vytvoří plán služby App Service. Toto je jako serverové farmy pro Azure webové aplikace. |
| [Vytvoření az webapp](https://docs.microsoft.com/cli/azure/webapp#create) | Vytvoří webové aplikace Azure. |
| [Konfigurace protokolu az webapp](https://docs.microsoft.com/cli/azure/webapp/log#config) | Nakonfiguruje protokoly, které se uchová webové aplikace Azure. |
| [AZ webapp protokolu stahování](https://docs.microsoft.com/cli/azure/webapp/log#download) | Soubory ke stažení do protokolů webové aplikace Azure do místního počítače. |

## <a name="next-steps"></a>Další kroky

Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Další ukázky skript aplikace služby rozhraní příkazového řádku najdete v [dokumentaci služby Azure App Service](../app-service-cli-samples.md).
