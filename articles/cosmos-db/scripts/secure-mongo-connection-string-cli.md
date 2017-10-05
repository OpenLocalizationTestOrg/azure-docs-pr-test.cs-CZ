---
title: "Azure CLI skriptu Get Azure Cosmos DB připojovací řetězec pro MongoDB aplikace | Microsoft Docs"
description: "Ukázka skriptu Azure CLI - Get Azure Cosmos DB připojovací řetězec pro MongoDB aplikace"
services: cosmos-db
documentationcenter: cosmosdb
author: mimig1
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: cosmos-db
ms.custom: mvc
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: cosmosdb
ms.workload: database
ms.date: 06/02/2017
ms.author: mimig
ms.openlocfilehash: 916c92cab39306352fdf9dff0e0685fd61832d16
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="get-an-azure-cosmos-db-connection-string-for-mongodb-apps-using-the-azure-cli"></a>Získat připojovací řetězec databázi Azure Cosmos pro MongoDB aplikace pomocí rozhraní příkazového řádku Azure

Tato ukázka získá připojovací řetězec databázi Cosmos Azure pro MongoDB aplikací pomocí rozhraní příkazového řádku Azure. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli-interactive[hlavní](../../../cli_scripts/cosmosdb/secure-cosmosdb-get-mongodb-connection-string/secure-cosmosdb-get-mongodb-connection-string.sh?highlight=36-39 "získání Cosmos DB Azure připojovací řetězec pro MongoDB aplikace")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení

Po spuštění ukázka skriptu, následující příkaz lze použít k odebrání skupiny prostředků a všechny prostředky, které jsou s ním spojená.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy. Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| Příkaz | Poznámky |
|---|---|
| [Vytvoření skupiny az](https://docs.microsoft.com/cli/azure/group#create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [AZ cosmosdb aktualizace](https://docs.microsoft.com/cli/azure/cosmosdb#update) | Aktualizuje účet Azure Cosmos DB. |
| [AZ cosmosdb seznamu připojení řetězců](https://docs.microsoft.com/cli/azure/cosmosdb#list-connection-strings) | Získá připojovací řetězec pro účet.|
| [Odstranění skupiny az](https://docs.microsoft.com/cli/azure/group#delete) | Odstraní skupinu prostředků, včetně všech vnořených prostředků. |

## <a name="next-steps"></a>Další kroky

Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Další ukázky skript příkazového řádku DB Cosmos Azure lze nalézt v [dokumentace Azure Cosmos DB CLI](../cli-samples.md).
