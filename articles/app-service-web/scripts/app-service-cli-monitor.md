---
title: "Ukázka skriptu rozhraní příkazového řádku - aaaAzure monitorování webové aplikace s protokoly webového serveru | Microsoft Docs"
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
ms.openlocfilehash: 012b800c03af77387bed3d098fae3c96d82aee29
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-a-web-app-with-web-server-logs"></a>Monitorování webové aplikace s protokoly webového serveru

V tomto scénáři vytvoříte skupinu prostředků, plán služby app service, webové aplikace a konfiguraci protokolů hello webové aplikace tooenable webového serveru. Pak si stáhnete hello souborů protokolu ke kontrole.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější. Spustit `az --version` toofind hello verze. Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/monitor-with-logs/monitor-with-logs.sh "Monitor Logs")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá hello následující příkazy toocreate skupinu prostředků, webové aplikace a všechny související prostředky. Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.

| Příkaz | Poznámky |
|---|---|
| [Vytvoření skupiny az](https://docs.microsoft.com/cli/azure/group#create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Vytvořit plán aplikační služby az](https://docs.microsoft.com/cli/azure/appservice/plan#create) | Vytvoří plán služby App Service. Toto je jako serverové farmy pro Azure webové aplikace. |
| [Vytvoření az webapp](https://docs.microsoft.com/cli/azure/webapp#create) | Vytvoří webové aplikace Azure. |
| [Konfigurace protokolu az webapp](https://docs.microsoft.com/cli/azure/webapp/log#config) | Nakonfiguruje protokoly, které se uchová webové aplikace Azure. |
| [AZ webapp protokolu stahování](https://docs.microsoft.com/cli/azure/webapp/log#download) | Hello stahování protokolů z hello tooyour aplikace Azure web pro místní počítač. |

## <a name="next-steps"></a>Další kroky

Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Další ukázky skriptu rozhraní příkazového řádku služby aplikace naleznete v hello [dokumentaci služby Azure App Service](../app-service-cli-samples.md).
