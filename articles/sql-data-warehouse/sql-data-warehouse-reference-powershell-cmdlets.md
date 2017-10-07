---
title: aaaPowerShell rutiny pro Azure SQL Data Warehouse
description: "Najít hello nejvyšší rutiny prostředí PowerShell pro Azure SQL Data Warehouse včetně jak toopause a obnovit databázi."
services: sql-data-warehouse
documentationcenter: NA
author: kevinvngo
manager: jhubbard
editor: 
ms.assetid: 6f0d5772-f05f-4cc8-9749-4adb153dfd50
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: reference
ms.date: 10/31/2016
ms.author: kevin;barbkess
ms.openlocfilehash: 84353b56131cf856e0724d338d7ed186fd2ceeaa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="powershell-cmdlets-and-rest-apis-for-sql-data-warehouse"></a>Rutiny prostředí PowerShell a rozhraní REST API pro SQL Data Warehouse
Mnoho úloh správy serveru SQL datového skladu lze spravovat pomocí rutin prostředí Azure PowerShell nebo rozhraní REST API.  Zde jsou některé příklady jak toouse prostředí PowerShell příkazy tooautomate běžné úlohy v SQL Data Warehouse.  Některé dobrými příklady REST, najdete v článku hello [spravovat škálovatelnost REST][Manage scalability with REST].

> [!NOTE]
> Pořadí toouse prostředí Azure PowerShell s SQL Data Warehouse, je nutné prostředí Azure PowerShell verze 1.0.3 nebo novější.  Vaše verze můžete zkontrolovat spuštěním **Get-Module - ListAvailable-Name Azure**.  je možné nainstalovat nejnovější verzi Hello od [instalačního programu webové platformy Microsoft][Microsoft Web Platform Installer].  Další informace o instalaci nejnovější verze hello najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell][How tooinstall and configure Azure PowerShell].
> 
> 

## <a name="get-started-with-azure-powershell-cmdlets"></a>Začínáme s rutinami prostředí Azure PowerShell
1. Otevřete Windows PowerShell.
2. Na příkazovém řádku prostředí PowerShell text hello spusťte tyto příkazy toosign v toohello Azure Resource Manager a vybrat své předplatné.
   
    ```PowerShell
    Login-AzureRmAccount
    Get-AzureRmSubscription
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

## <a name="pause-sql-data-warehouse-example"></a>Pozastavení SQL Data Warehouse příklad
Pozastavení databáze s názvem "Database02", které jsou hostované na serveru s názvem "Server01."  Hello server je ve skupině prostředků Azure s názvem "ResourceGroup1."

```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
```
Variace, v tomto příkladu prostřednictvím kanálu předá objekt hello načíst příliš[Suspend-AzureRmSqlDatabase][Suspend-AzureRmSqlDatabase].  V důsledku toho se pozastaví hello databáze. poslední příkaz Hello zobrazuje výsledky hello.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```

## <a name="start-sql-data-warehouse-example"></a>Spusťte SQL Data Warehouse příklad
Operace obnovení databáze s názvem "Database02", které jsou hostované na serveru s názvem "Server01." Hello server je obsažen ve skupině prostředků s názvem "ResourceGroup1."

```Powershell
Resume-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" -DatabaseName "Database02"
```

Variace, načte tento příklad databáze s názvem "Database02" ze serveru s názvem "Server01", který je obsažen ve skupině prostředků s názvem "ResourceGroup1." Ji prostřednictvím kanálu předá objekt hello načíst příliš[Resume-AzureRmSqlDatabase][Resume-AzureRmSqlDatabase].

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
```

> [!NOTE]
> Všimněte si, že pokud je váš server foo.database.windows.net, použijte "foo" jak hello - ServerName hello rutiny prostředí PowerShell.
> 
> 

## <a name="other-supported-powershell-cmdlets"></a>Ostatní podporované rutiny prostředí PowerShell
Tyto rutiny prostředí PowerShell jsou podporovány službou Azure SQL Data Warehouse.

* [Get-AzureRmSqlDatabase][Get-AzureRmSqlDatabase]
* [Get-AzureRmSqlDeletedDatabaseBackup][Get-AzureRmSqlDeletedDatabaseBackup]
* [Get-AzureRmSqlDatabaseRestorePoints][Get-AzureRmSqlDatabaseRestorePoints]
* [Nový AzureRmSqlDatabase][New-AzureRmSqlDatabase]
* [Remove-AzureRmSqlDatabase][Remove-AzureRmSqlDatabase]
* [Obnovení AzureRmSqlDatabase][Restore-AzureRmSqlDatabase]
* [Resume-AzureRmSqlDatabase][Resume-AzureRmSqlDatabase]
* [Select-AzureRmSubscription][Select-AzureRmSubscription]
* [Set-AzureRmSqlDatabase][Set-AzureRmSqlDatabase]
* [Suspend-AzureRmSqlDatabase][Suspend-AzureRmSqlDatabase]

## <a name="next-steps"></a>Další kroky
Další příklady prostředí PowerShell najdete v tématu:

* [Vytvoření SQL Data Warehouse pomocí prostředí PowerShell][Create a SQL Data Warehouse using PowerShell]
* [Obnovení databáze][Database restore]

Další úlohy, které je možné automatizovat pomocí prostředí PowerShell, najdete v části [rutiny databáze SQL Azure][Azure SQL Database Cmdlets]. Všimněte si, že ne všechny rutiny Azure SQL Database jsou podporovány pro Azure SQL Data Warehouse.  Seznam úloh, které je možné automatizovat se zbytkem najdete v tématu [operace u databází SQL Azure][Operations for Azure SQL Databases].

<!--Image references-->

<!--Article references-->
[How tooinstall and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
[Create a SQL Data Warehouse using PowerShell]: ./sql-data-warehouse-get-started-provision-powershell.md
[Database restore]: ./sql-data-warehouse-restore-database-powershell.md
[Manage scalability with REST]: ./sql-data-warehouse-manage-compute-rest-api.md

<!--MSDN references-->
[Azure SQL Database Cmdlets]: https://msdn.microsoft.com/library/mt574084.aspx
[Operations for Azure SQL Databases]: https://msdn.microsoft.com/library/azure/dn505719.aspx
[Get-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt603648.aspx
[Get-AzureRmSqlDeletedDatabaseBackup]: https://msdn.microsoft.com/library/mt693387.aspx
[Get-AzureRmSqlDatabaseRestorePoints]: https://msdn.microsoft.com/library/mt603642.aspx
[New-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619339.aspx
[Remove-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619368.aspx
[Restore-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx
[Resume-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619347.aspx
<!-- It appears that Select-AzureRmSubscription isn't documented, so this points tooSelect-AzureSubscription -->
[Select-AzureRmSubscription]: https://msdn.microsoft.com/library/dn722499.aspx
[Set-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619433.aspx
[Suspend-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619337.aspx

<!--Other Web references-->
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
