---
title: "rozhraní příkazového řádku skriptu – vytvoření brány firewall pro Azure Cosmos DB aaaAzure | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – vytvoření brány firewall pro Azure Cosmos DB"
services: cosmos-db
documentationcenter: cosmosdb
author: mimig1
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: cosmos-db
ms.custom: sammvcple
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: cosmosdb
ms.workload: database
ms.date: 06/02/2017
ms.author: mimig
ms.openlocfilehash: d4bee4f37906033c96826b9662d2ba396325c792
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-create-a-firewall-using-hello-azure-cli"></a>Azure Cosmos DB: Vytvoření brány firewall pomocí hello rozhraní příkazového řádku Azure

Tento ukázkový skript rozhraní příkazového řádku vytvoří zásady brány firewall pro jakýkoli druh účtu Azure Cosmos DB. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější. Spustit `az --version` toofind hello verze. Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli-interactive[main](../../../cli_scripts/cosmosdb/secure-cosmosdb-create-firewall/secure-cosmosdb-create-firewall.sh?highlight=38-42 "Create an Azure Cosmos DB firewall")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení

Po spuštění ukázka skriptu hello hello následující příkaz může být skupiny prostředků použít tooremove hello a všechny prostředky, které jsou s ním spojená.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá hello následující příkazy. Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.

| Příkaz | Poznámky |
|---|---|
| [Vytvoření skupiny az](https://docs.microsoft.com/cli/azure/group#create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Vytvoření az cosmosdb](https://docs.microsoft.com/cli/azure/cosmosdb#create) | Vytvoří účet Azure CosmosDB. |
| [AZ cosmosdb aktualizace](https://docs.microsoft.com/cli/azure/cosmosdb#update) | Aktualizace nastavení služby Azure CosmosDB tooinclude účtu brány firewall. |
| [Odstranění skupiny az](https://docs.microsoft.com/cli/azure/group#delete) | Odstraní skupinu prostředků, včetně všech vnořených prostředků. |

## <a name="next-steps"></a>Další kroky

Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Další ukázky skript příkazového řádku DB Cosmos Azure lze nalézt v hello [dokumentace Azure Cosmos DB CLI](../cli-samples.md).
