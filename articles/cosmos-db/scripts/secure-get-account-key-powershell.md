---
title: "aaaAzure skript prostředí PowerShell-Get účet klíče pro cosmosdb | Microsoft Docs"
description: "Azure ukázkový skript prostředí PowerShell - Get klíče účtu pro cosmosdb"
services: cosmos-db
documentationcenter: cosmosdb
author: mimig1
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: cosmos-db
ms.custom: mvc
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: cosmosdb
ms.workload: database
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: 9ccee3085dc4fa6507d43e4a220dd5fc32889a9b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-account-keys-for-azure-cosmos-db-using-powershell"></a>Získání klíče účtu pro Azure DB Cosmos pomocí prostředí PowerShell

Tato ukázka získá klíče účtu pro jakýkoli druh účtu Azure Cosmos DB.  

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-powershell[main](../../../powershell_scripts/cosmosdb/get-account-keys/get-account-keys.ps1?highlight=36-40 "Get hello keys for an Azure Cosmos DB account")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení

Po spuštění ukázka skriptu hello hello následující příkaz může být skupiny prostředků použít tooremove hello a všechny prostředky, které jsou s ním spojená.

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá hello následující příkazy. Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.

| Příkaz | Poznámky |
|---|---|
| [Nový AzureRmResourceGroup](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/new-azurermresourcegroup) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Nové AzureRmResource](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresource?view=azurermps-3.8.0) | Vytvoří logického serveru, který je hostitelem databáze nebo elastického fondu. |
| [Vyvolání AzureRmResourceAction](https://docs.microsoft.com/powershell/module/azurerm.resources/invoke-azurermresourceaction?view=azurermps-3.8.0) | Vyvolá akce na hello Azure CosmosDB účtu. |
| [Remove-AzureRmResourceGroup](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/remove-azurermresourcegroup) | Odstraní skupinu prostředků, včetně všech vnořených prostředků. |
|||

## <a name="next-steps"></a>Další kroky

Další informace o hello prostředí Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](https://docs.microsoft.com/powershell/).

Další ukázky skriptu Azure Cosmos DB PowerShell lze nalézt v hello [skriptů prostředí PowerShell DB Cosmos Azure](../powershell-samples.md).
