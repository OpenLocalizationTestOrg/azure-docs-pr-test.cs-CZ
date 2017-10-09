---
title: "aaaCreate SQL Data Warehouse pomocí TSQL | Microsoft Docs"
description: "Zjistěte, jak toocreate Azure SQL datového skladu pomocí TSQL"
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: jhubbard
editor: 
tags: azure-sql-data-warehouse
ms.assetid: a4e2e68e-aa9c-4dd3-abb0-f7df997d237a
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: create
ms.date: 10/31/2016
ms.author: elbutter;barbkess
ms.openlocfilehash: 81ef59a66c61452ff8a2aca29837f155e87d017d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-sql-data-warehouse-database-by-using-transact-sql-tsql"></a>Vytvoření databáze SQL Data Warehouse pomocí jazyka Transact-SQL (TSQL)
> [!div class="op_single_selector"]
> * [Azure Portal](sql-data-warehouse-get-started-provision.md)
> * [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
> * [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)
>
>

Tento článek ukazuje, jak toocreate SQL datového skladu pomocí T-SQL.

## <a name="prerequisites"></a>Požadavky
tooget spuštění, budete potřebovat:

* **Účet Azure**: navštivte [bezplatná zkušební verze Azure] [ Azure Free Trial] nebo [kredity Azure MSDN] [ MSDN Azure Credits] toocreate účet.
* **Azure SQL server**: Viz [vytvoření logického serveru Azure SQL Database s hello portálu Azure] [vytvoření logického serveru Azure SQL Database pomocí portálu Azure hello] nebo [vytvoření logického serveru Azure SQL Database pomocí prostředí PowerShell] [vytvoření Azure SQL Logický server databáze pomocí prostředí PowerShell] Další podrobnosti.
* **Skupina prostředků**: použijte hello stejný zdroj skupiny jako server Azure SQL nebo v tématu [jak toocreate skupinu prostředků][how toocreate a resource group].
* **Prostředí tooexecute T-SQL**: můžete použít [Visual Studio][Installing Visual Studio and SSDT], [sqlcmd][sqlcmd], nebo [SSMS] [ SSMS] tooexecute T-SQL.

> [!NOTE]
> Vytvoření služby SQL Data Warehouse může znamenat, že se vám začne fakturovat nová služba.  Další podrobnosti o cenách najdete v tématu [SQL Data Warehouse – ceny][SQL Data Warehouse pricing].
>
>

## <a name="create-a-database-with-visual-studio"></a>Vytvoření databáze pomocí sady Visual Studio
Pokud jste nový tooVisual Studio, najdete v článku hello [dotazu Azure SQL Data Warehouse (Visual Studio)][Query Azure SQL Data Warehouse (Visual Studio)].  toostart, otevřete Průzkumník objektů systému SQL Server v sadě Visual Studio a připojte toohello serveru, který bude hostitelem databáze SQL Data Warehouse.  Po připojení můžete vytvořit SQL Data Warehouse spuštěním následujícího příkazu SQL hello hello **hlavní** databáze.  Tento příkaz vytvoří hello databázi MySqlDwDb s cílem služby DW400 a povolit hello databáze toogrow tooa maximální velikosti 10 TB.

```sql
CREATE DATABASE MySqlDwDb COLLATE SQL_Latin1_General_CP1_CI_AS (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB);
```

## <a name="create-a-database-with-sqlcmd"></a>Vytvoření databáze pomocí sqlcmd
Alternativně můžete spustit stejný příkaz pomocí sqlcmd spuštěním hello následující na příkazovém řádku hello.

```sql
sqlcmd -S <Server Name>.database.windows.net -I -U <User> -P <Password> -Q "CREATE DATABASE MySqlDwDb COLLATE SQL_Latin1_General_CP1_CI_AS (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB)"
```

Hello výchozí kolace není-li zadána je COLLATE SQL_Latin1_General_CP1_CI_AS.  Hello `MAXSIZE` může být v rozmezí 250 GB do 240 TB.  Hello `SERVICE_OBJECTIVE` může být v rozmezí od DW100 do DW2000 [DWU][DWU].  Seznam všech platných hodnot najdete v tématu MSDN dokumentaci hello [CREATE DATABASE][CREATE DATABASE].  Hello MAXSIZE a SERVICE_OBJECTIVE může být změněna pomocí [ALTER DATABASE] [ ALTER DATABASE] příkazů T-SQL.  Hello kolaci databáze nelze změnit po vytvoření.   Upozornění: je třeba použít při změna hello SERVICE_OBJECTIVE jako změna DWU způsobí restartování služeb, která zruší všechny dotazy na cestě.  Změna hodnoty parametru MAXSIZE nerestartuje služby, protože jde pouze o operaci s metadaty.

## <a name="next-steps"></a>Další kroky
Po dokončení zřizování, můžete svoji službu SQL Data Warehouse [načíst ukázková data] [ load sample data] nebo podívat, jak příliš[vyvíjet][develop], [načíst][load], nebo [migrovat][migrate].

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md
[how toocreate a SQL Data Warehouse from hello Azure portal]: sql-data-warehouse-get-started-provision.md
[Query Azure SQL Data Warehouse (Visual Studio)]: sql-data-warehouse-query-visual-studio.md
[migrate]: sql-data-warehouse-overview-migrate.md
[develop]: sql-data-warehouse-overview-develop.md
[load]: sql-data-warehouse-overview-load.md
[load sample data]: sql-data-warehouse-load-sample-databases.md
[Create an Azure SQL database with hello Azure Portal]: ../sql-database/sql-database-get-started.md
[Create an Azure SQL database with PowerShell]: ../sql-database/sql-database-create-and-configure-database-powershell
[how toocreate a resource group]: ../azure-resource-manager/resource-group-template-deploy-portal.md#create-resource-group
[Installing Visual Studio and SSDT]: sql-data-warehouse-install-visual-studio.md
[sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md

<!--MSDN references-->
[CREATE DATABASE]: https://msdn.microsoft.com/library/mt204021.aspx
[ALTER DATABASE]: https://msdn.microsoft.com/library/mt204042.aspx
[SSMS]: https://msdn.microsoft.com/library/mt238290.aspx

<!--Other Web references-->
[SQL Data Warehouse pricing]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure Free Trial]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN Azure Credits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F
