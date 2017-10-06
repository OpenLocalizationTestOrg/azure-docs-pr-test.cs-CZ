---
title: "aaaCreate funkce aplikace a nasazení kódu funkce z Githubu | Microsoft Docs"
description: "Ukázka skriptu Azure CLI – vytvoření funkce aplikace a nasazení kódu funkce z Githubu"
services: functions
keywords: 
author: syntaxc4
ms.author: cfowler
ms.date: 04/27/2017
ms.topic: sample
ms.service: functions
ms.custom: mvc
ms.openlocfilehash: 026886f11909149db695d9a52d0aa37f109f64e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-function-app-and-deploy-function-code-from-github"></a>Vytvoření funkce aplikace a nasazení funkce kód z Githubu

Tento ukázkový skript vytvoří aplikaci funkce pomocí hello [plánu spotřeby](../functions-scale.md#consumption-plan) s související prostředky a nasadí funkce kódu z veřejného úložiště GitHub (bez průběžné nasazování). Průběžné doručování kód funkce z Githubu, najdete v tématu [vytvořit aplikaci funkce a průběžně nasadit z Githubu](functions-cli-create-function-app-github-continuous.md)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější. Spustit `az --version` toofind hello verze. Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Ukázkový skript

Tato ukázka funkce Azure aplikace vytvoří a nasadí kód funkce z Githubu.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/deploy-function-app-with-function-github/deploy-function-app-with-function-github.sh?highlight=3 "Create a function app with deployment from GitHub")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Vysvětlení skriptu

Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci. Tento skript používá hello následující příkazy:

| Příkaz | Poznámky |
|---|---|
| [Vytvoření skupiny az](https://docs.microsoft.com/cli/azure/group#create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Vytvořit účet úložiště az](https://docs.microsoft.com/cli/azure/appservice/plan#create) | Vytvoří plán služby App Service. |
| [Vytvoření az functionapp](https://docs.microsoft.com/cli/azure/appservice/web#delete) | Vytvoří aplikaci Azure funkce. |
| [Konfigurace správy zdrojového kódu webové služby App Service az](https://docs.microsoft.com/cli/azure/appservice/web/source-control#config) | Přidruží aplikaci funkce Git nebo Mercurial úložiště. |

## <a name="next-steps"></a>Další kroky

Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Další ukázky skriptu rozhraní příkazového řádku funkce Azure lze nalézt v hello [dokumentace Azure Functions](../functions-cli-samples.md).
