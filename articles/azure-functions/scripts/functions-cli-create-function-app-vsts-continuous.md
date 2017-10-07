---
title: "aaaCreate funkce aplikace a nasazení kódu funkce z Visual Studio Team Services | Microsoft Docs"
description: "Vytvoření funkce aplikace a nasazení funkce kód z Visual Studio Team Services"
services: functions
keywords: 
author: syntaxc4
ms.author: cfowler
ms.date: 04/28/2017
ms.topic: sample
ms.service: functions
ms.custom: mvc
ms.openlocfilehash: 774bee73025cc9ac46f8b2a6c10edbfa3c2d353b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-app-service"></a>Vytvoření služby aplikace

V tomto scénáři se dozvíte, jak funkce aplikace pomocí toocreate hello [plánu spotřeby](../functions-scale.md#consumption-plan) s jejími související prostředky a průběžně nasadí funkce kódu z Visual Studio Team Services (VSTS) úložiště. V této ukázce budete potřebovat:

* Služby VSTS úložiště s kódem funkce, které mají oprávnění pro správu.
* A [Personal Access Token (Jan)](https://help.github.com/articles/creating-an-access-token-for-command-line-use) pro váš účet GitHub.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější. Spustit `az --version` toofind hello verze. Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Ukázkový skript

Tato ukázka funkce Azure aplikace vytvoří a nasadí funkce kód z Visual Studio Team Services.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/deploy-function-app-with-function-vsts/deploy-function-app-with-function-vsts.sh?highlight=3-4 "Azure Service")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá hello následující příkazy toocreate skupinu prostředků, webové aplikace, documentdb a všechny související prostředky. Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.

| Příkaz | Poznámky |
|---|---|
| [Vytvoření skupiny az](https://docs.microsoft.com/cli/azure/group#create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Vytvořit účet úložiště az](https://docs.microsoft.com/cli/azure/appservice/plan#create) | Vytvoří plán služby App Service. |
| [Vytvoření az functionapp](https://docs.microsoft.com/cli/azure/appservice/web#delete) |
| [Konfigurace správy zdrojového kódu webové služby App Service az](https://docs.microsoft.com/cli/azure/appservice/web/source-control#config) | Přidruží aplikaci funkce Git nebo Mercurial úložiště. |

## <a name="next-steps"></a>Další kroky

Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Další ukázky skriptu rozhraní příkazového řádku funkce Azure lze nalézt v hello [dokumentace Azure Functions](../functions-cli-samples.md).
