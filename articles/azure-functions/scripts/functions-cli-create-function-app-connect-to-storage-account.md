---
title: "aaaCreate funkce Azure, která se připojuje tooan Azure Storage | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – vytvoření funkce Azure, která se připojuje tooan Azure Storage"
services: functions
documentationcenter: functions
author: rachelappel
manager: erikre
editor: 
tags: functions
ms.assetid: 
ms.service: functions
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 04/20/2017
ms.author: rachelap
ms.custom: mvc
ms.openlocfilehash: a51a2c17149478eb2d3d0d4034400ed00cd8416c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-function-app-into-azure-storage-account"></a>Integrace funkce aplikace do účtu úložiště Azure

Tento ukázkový skript vytvoří aplikaci funkce a účet úložiště.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější. Spustit `az --version` toofind hello verze. Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Ukázkový skript

Tato ukázka vytvoří aplikaci funkce Azure a přidá hello úložiště připojovací řetězec tooan nastavení aplikace.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-connect-to-storage/create-function-app-connect-to-storage-account.sh "Integrate Function App into Azure Storage Account")]


## <a name="clean-up-deployment"></a>Vyčištění nasazení

Po spuštění ukázka skriptu hello hello následující příkaz může být použité tooremove hello prostředků skupina, aplikační služby a všechny související prostředky:

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá hello následující příkazy. Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.

| Příkaz | Poznámky |
|---|---|
| [AZ přihlášení](https://docs.microsoft.com/cli/azure/#login) | TooAzure přihlášení. |
| [Vytvoření skupiny az](https://docs.microsoft.com/cli/azure/group#create) | Vytvořte skupinu prostředků s umístěním |
| [Vytvořit účet úložiště az](https://docs.microsoft.com/cli/azure/storage/account) | vytvořit účet úložiště |
| [Vytvoření az functionapp](https://docs.microsoft.com/cli/azure/functionapp#create) | Vytvořit novou aplikaci funkce |
| [Odstranění skupiny az](https://docs.microsoft.com/cli/azure/group#delete) | Vyčištění |

## <a name="next-steps"></a>Další kroky

Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Další ukázky skriptu rozhraní příkazového řádku funkce Azure lze nalézt v hello [dokumentace Azure Functions](../functions-cli-samples.md).
