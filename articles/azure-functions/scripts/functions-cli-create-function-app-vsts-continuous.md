---
title: "Vytvoření funkce aplikace a nasazení funkce kód z Visual Studio Team Services | Microsoft Docs"
description: "Vytvoření funkce aplikace a nasazení funkce kód z Visual Studio Team Services"
services: functions
keywords: 
author: syntaxc4
ms.author: cfowler
ms.date: 04/28/2017
ms.topic: sample
ms.service: functions
ms.custom: mvc
ms.openlocfilehash: 2ef177b55ad7ffd351156821f429e6ff8fbeccc7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-app-service"></a>Vytvoření služby aplikace

V tomto scénáři se dozvíte, jak vytvořit funkce aplikace pomocí [plánu spotřeby](../functions-scale.md#consumption-plan) s jejími související prostředky a průběžně nasadí funkce kódu z úložiště Visual Studio Team Services (VSTS). V této ukázce budete potřebovat:

* Služby VSTS úložiště s kódem funkce, které mají oprávnění pro správu.
* A [Personal Access Token (Jan)](https://help.github.com/articles/creating-an-access-token-for-command-line-use) pro váš účet GitHub.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Ukázkový skript

Tato ukázka funkce Azure aplikace vytvoří a nasadí funkce kód z Visual Studio Team Services.

[!code-azurecli-interactive[hlavní](../../../cli_scripts/azure-functions/deploy-function-app-with-function-vsts/deploy-function-app-with-function-vsts.sh?highlight=3-4 "služby Azure")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy k vytvoření skupiny prostředků, webové aplikace, documentdb a všechny související prostředky. Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| Příkaz | Poznámky |
|---|---|
| [Vytvoření skupiny az](https://docs.microsoft.com/cli/azure/group#create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Vytvořit účet úložiště az](https://docs.microsoft.com/cli/azure/appservice/plan#create) | Vytvoří plán služby App Service. |
| [Vytvoření az functionapp](https://docs.microsoft.com/cli/azure/appservice/web#delete) |
| [Konfigurace správy zdrojového kódu webové služby App Service az](https://docs.microsoft.com/cli/azure/appservice/web/source-control#config) | Přidruží aplikaci funkce Git nebo Mercurial úložiště. |

## <a name="next-steps"></a>Další kroky

Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Další ukázky skriptu rozhraní příkazového řádku funkce Azure lze nalézt v [dokumentace Azure Functions](../functions-cli-samples.md).
