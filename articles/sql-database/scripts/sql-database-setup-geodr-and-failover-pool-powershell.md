---
title: "aaaPowerShell příklad aktivní ve fondu geografická replikace Azure SQL database | Microsoft Docs"
description: "Azure PowerShell příklad skriptu tooset až aktivní geografickou replikací pro ve fondu Azure SQL database"
services: sql-database
documentationcenter: sql-database
author: CarlRabeler
manager: jhubbard
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: business continuity
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 07/25/2017
ms.author: carlrab
ms.openlocfilehash: 9d183f08dcc07ba864e42fe70a562fef8bd572f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-tooconfigure-active-geo-replication-for-a-pooled-azure-sql-database"></a>Pomocí prostředí PowerShell tooconfigure active geografická replikace pro databázi Azure SQL ve fondu

Tento ukázkový skript prostředí PowerShell nakonfiguruje aktivní geografickou replikací pro databázi Azure SQL v elastickém fondu a převezme toohello sekundární replika databáze Azure SQL hello.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-scripts"></a>Ukázkové skripty

[!code-powershell[main](../../../powershell_scripts/sql-database/setup-geodr-and-failover-pool/setup-geodr-and-failover-pool.ps1?highlight=16-19 "Set up active geo-replication for elastic pool")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení

Po spuštění ukázka skriptu hello hello následující příkaz může být skupiny prostředků použít tooremove hello a všechny prostředky, které jsou s ním spojená.

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myPrimaryResourceGroup"
Remove-AzureRmResourceGroup -ResourceGroupName "mySecondaryResourceGroup"
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá hello následující příkazy. Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.

| Příkaz | Poznámky |
|---|---|
| [Nový AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Nový AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver) | Vytvoří logického serveru, který je hostitelem databáze nebo elastického fondu. |
| [Nový AzureRmSqlElasticPool](/powershell/module/azurerm.sql/new-azurermsqlelasticpool) | Vytvoří elastického fondu v rámci logického serveru. |
| [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) | Vytvoří databázi logického serveru jako jednu, nebo databázi ve fondu. |
| [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase) | Aktualizuje vlastnosti databáze nebo přesouvá databázi do, mimo nebo mezi elastické fondy. |
| [Nové AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/new-azurermsqldatabasesecondary)| Sekundární databáze pro databázi vytvoří a spustí replikaci dat. |
| [Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase)| Získá jednu nebo více databází. |
| [Set-AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/set-azurermsqldatabasesecondary)| Přepne toobe primární sekundární databáze v pořadí tooinitiate převzetí služeb při selhání.|
| [Get-AzureRmSqlDatabaseReplicationLink](/powershell/module/azurerm.sql/get-azurermsqldatabasereplicationlink) | Získá hello geografická replikace propojení mezi Azure SQL Database a skupinu prostředků nebo SQL Server. |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Odstraní skupinu prostředků, včetně všech vnořených prostředků. |
|||

## <a name="next-steps"></a>Další kroky

Další informace o hello prostředí Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).

Další ukázky skriptu PowerShell databáze SQL naleznete v hello [skriptů prostředí PowerShell databáze SQL Azure](../sql-database-powershell-samples.md).
