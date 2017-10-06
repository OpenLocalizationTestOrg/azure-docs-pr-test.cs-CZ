---
title: "aaaCreate SQL Data Warehouse pomocí prostředí PowerShell | Microsoft Docs"
description: "Vytvoření SQL Data Warehouse pomocí prostředí PowerShell"
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: jhubbard
editor: 
ms.assetid: 97434863-7938-4129-8949-5a119f5949e3
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: create
ms.date: 10/31/2016
ms.author: elbutter;barbkess
ms.openlocfilehash: d8af29ec285a11285785ab5474e4dfc8c36bc3ab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-sql-data-warehouse-using-powershell"></a>Vytvoření SQL Data Warehouse pomocí prostředí PowerShell
> [!div class="op_single_selector"]
> * [Azure Portal](sql-data-warehouse-get-started-provision.md)
> * [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
> * [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)
>
>

Tento článek ukazuje, jak toocreate SQL datového skladu pomocí prostředí PowerShell.

## <a name="prerequisites"></a>Požadavky
tooget spuštění, budete potřebovat:

* **Účet Azure**: navštivte [bezplatná zkušební verze Azure] [ Azure Free Trial] nebo [kredity Azure MSDN] [ MSDN Azure Credits] toocreate účet.
* **Azure SQL server**: najdete v části [vytvoření Azure SQL database v hello portálu Azure] [ Create an Azure SQL database in hello Azure Portal] nebo [vytvoření Azure SQL database pomocí prostředí PowerShell] [ Create an Azure SQL database with PowerShell] další podrobnosti.
* **Skupina prostředků**: použijte hello stejný zdroj skupiny jako server Azure SQL nebo v tématu [jak toocreate skupinu prostředků](../azure-resource-manager/resource-group-portal.md).
* **Prostředí PowerShell verze 1.0.3 nebo novější:** To, jakou máte verzi, můžete zjistit spuštěním rutiny **Get-Module -ListAvailable -Name Azure**.  je možné nainstalovat nejnovější verzi Hello od [instalačního programu webové platformy Microsoft][Microsoft Web Platform Installer].  Další informace o instalaci nejnovější verze hello najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell][How tooinstall and configure Azure PowerShell].

> [!NOTE]
> Vytvoření služby SQL Data Warehouse může znamenat, že se vám začne fakturovat nová služba.  Další podrobnosti o cenách najdete v tématu [SQL Data Warehouse – ceny][SQL Data Warehouse pricing].
>
>

## <a name="create-a-sql-data-warehouse"></a>Vytvoření SQL Data Warehouse
1. Otevřete Windows PowerShell.
2. Spusťte tuto rutinu toologin tooAzure Resource Manager.

    ```Powershell
    Login-AzureRmAccount
    ```
3. Vyberte předplatné hello chcete toouse pro aktuální relaci.

    ```Powershell
    Get-AzureRmSubscription    -SubscriptionName "MySubscription" | Select-AzureRmSubscription
    ```
4. Vytvořte databázi. Tento příklad vytvoří databáze s názvem "mynewsqldw", s úrovní cíle služby "DW400" toohello serveru s názvem "sqldwserver1", který je ve skupině prostředků hello s názvem "mywesteuroperesgp1".

   ```Powershell
   New-AzureRmSqlDatabase -RequestedServiceObjectiveName "DW400" -DatabaseName "mynewsqldw" -ServerName "sqldwserver1" -ResourceGroupName "mywesteuroperesgp1" -Edition "DataWarehouse" -CollationName "SQL_Latin1_General_CP1_CI_AS" -MaxSizeBytes 10995116277760
   ```

Požadované parametry jsou:

* **RequestedServiceObjectiveName**: hello množství [DWU] [ DWU] jste požádali.  Podporované hodnoty: DW100, DW200, DW300, DW400, DW500, DW600, DW1000, DW1200, DW1500, DW2000, DW3000 a DW6000.
* **DatabaseName**: název hello hello SQL Data Warehouse, kterou vytváříte.
* **ServerName**: název hello hello serveru, který používáte pro vytvoření (musí být verze 12).
* **ResourceGroupName**: Skupina prostředků, kterou používáte.  skupiny toofind dostupných prostředků v rámci vašeho předplatného použijte rutinu Get-AzureResource.
* **Edice**: musí být "Datového skladu" toocreate SQL Data Warehouse.

Volitelné parametry jsou:

* **%{Collationname/**: hello výchozí kolace není-li zadána, je SQL_Latin1_General_CP1_CI_AS.  Kolaci nejde pro databázi změnit.
* **MaxSizeBytes**: hello výchozí maximální velikost databáze je 10 GB.

Další podrobnosti o možnostech parametr hello najdete v tématu [New-AzureRmSqlDatabase] [ New-AzureRmSqlDatabase] a [Create Database (Azure SQL Data Warehouse)][Create Database (Azure SQL Data Warehouse)].

## <a name="next-steps"></a>Další kroky
Po dokončení SQL Data Warehouse zřizování, můžete chtít tootry [načíst ukázková data] [ loading sample data] nebo podívat, jak příliš[vyvíjet] [ develop] , [načíst][load], nebo [migrovat][migrate].

Pokud vás zajímají další informace o tom toomanage SQL Data Warehouse prostřednictvím kódu programu, podívejte se na náš článek o toouse [rutiny prostředí PowerShell a rozhraní REST API][PowerShell cmdlets and REST APIs].

<!--Image references-->

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md
[migrate]: ./sql-data-warehouse-overview-migrate.md
[develop]: ./sql-data-warehouse-overview-develop.md
[load]: ./sql-data-warehouse-load-with-bcp.md
[loading sample data]: ./sql-data-warehouse-load-sample-databases.md
[PowerShell cmdlets and REST APIs]: ./sql-data-warehouse-reference-powershell-cmdlets.md
[firewall rules]: ../sql-database-configure-firewall-settings.md

[How tooinstall and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
[how toocreate a SQL Data Warehouse from hello Azure Portal]: ./sql-data-warehouse-get-started-provision.md
[Create an Azure SQL database in hello Azure Portal]: ../sql-database/sql-database-get-started.md
[Create an Azure SQL database with PowerShell]: ../sql-database/sql-database-get-started-powershell.md
[how toocreate a resource group]: ../azure-resource-manager/resource-group-template-deploy-portal.md#create-resource-group

<!--MSDN references-->
[MSDN]: https://msdn.microsoft.com/library/azure/dn546722.aspx
[New-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619339.aspx
[Create Database (Azure SQL Data Warehouse)]: https://msdn.microsoft.com/library/mt204021.aspx

<!--Other Web references-->
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
[SQL Data Warehouse pricing]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure Free Trial]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN Azure Credits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F
