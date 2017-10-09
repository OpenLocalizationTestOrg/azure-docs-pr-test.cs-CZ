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
# <a name="create-a-sql-data-warehouse-database-by-using-transact-sql-tsql"></a><span data-ttu-id="2368d-103">Vytvoření databáze SQL Data Warehouse pomocí jazyka Transact-SQL (TSQL)</span><span class="sxs-lookup"><span data-stu-id="2368d-103">Create a SQL Data Warehouse database by using Transact-SQL (TSQL)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="2368d-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="2368d-104">Azure Portal</span></span>](sql-data-warehouse-get-started-provision.md)
> * [<span data-ttu-id="2368d-105">TSQL</span><span class="sxs-lookup"><span data-stu-id="2368d-105">TSQL</span></span>](sql-data-warehouse-get-started-create-database-tsql.md)
> * [<span data-ttu-id="2368d-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="2368d-106">PowerShell</span></span>](sql-data-warehouse-get-started-provision-powershell.md)
>
>

<span data-ttu-id="2368d-107">Tento článek ukazuje, jak toocreate SQL datového skladu pomocí T-SQL.</span><span class="sxs-lookup"><span data-stu-id="2368d-107">This article shows you how toocreate a SQL Data Warehouse using T-SQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2368d-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2368d-108">Prerequisites</span></span>
<span data-ttu-id="2368d-109">tooget spuštění, budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="2368d-109">tooget started, you need:</span></span>

* <span data-ttu-id="2368d-110">**Účet Azure**: navštivte [bezplatná zkušební verze Azure] [ Azure Free Trial] nebo [kredity Azure MSDN] [ MSDN Azure Credits] toocreate účet.</span><span class="sxs-lookup"><span data-stu-id="2368d-110">**Azure account**: Visit [Azure Free Trial][Azure Free Trial] or [MSDN Azure Credits][MSDN Azure Credits] toocreate an account.</span></span>
* <span data-ttu-id="2368d-111">**Azure SQL server**: Viz [vytvoření logického serveru Azure SQL Database s hello portálu Azure] [vytvoření logického serveru Azure SQL Database pomocí portálu Azure hello] nebo [vytvoření logického serveru Azure SQL Database pomocí prostředí PowerShell] [vytvoření Azure SQL Logický server databáze pomocí prostředí PowerShell] Další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="2368d-111">**Azure SQL server**:  See [Create an Azure SQL Database logical server with hello Azure Portal][Create an Azure SQL Database logical server with hello Azure Portal] or [Create an Azure SQL Database logical server with PowerShell][Create an Azure SQL Database logical server with PowerShell] for more details.</span></span>
* <span data-ttu-id="2368d-112">**Skupina prostředků**: použijte hello stejný zdroj skupiny jako server Azure SQL nebo v tématu [jak toocreate skupinu prostředků][how toocreate a resource group].</span><span class="sxs-lookup"><span data-stu-id="2368d-112">**Resource group**: Either use hello same resource group as your Azure SQL server or see [how toocreate a resource group][how toocreate a resource group].</span></span>
* <span data-ttu-id="2368d-113">**Prostředí tooexecute T-SQL**: můžete použít [Visual Studio][Installing Visual Studio and SSDT], [sqlcmd][sqlcmd], nebo [SSMS] [ SSMS] tooexecute T-SQL.</span><span class="sxs-lookup"><span data-stu-id="2368d-113">**Environment tooexecute T-SQL**: You can use [Visual Studio][Installing Visual Studio and SSDT], [sqlcmd][sqlcmd], or [SSMS][SSMS] tooexecute T-SQL.</span></span>

> [!NOTE]
> <span data-ttu-id="2368d-114">Vytvoření služby SQL Data Warehouse může znamenat, že se vám začne fakturovat nová služba.</span><span class="sxs-lookup"><span data-stu-id="2368d-114">Creating a SQL Data Warehouse may result in a new billable service.</span></span>  <span data-ttu-id="2368d-115">Další podrobnosti o cenách najdete v tématu [SQL Data Warehouse – ceny][SQL Data Warehouse pricing].</span><span class="sxs-lookup"><span data-stu-id="2368d-115">See [SQL Data Warehouse pricing][SQL Data Warehouse pricing] for more details on pricing.</span></span>
>
>

## <a name="create-a-database-with-visual-studio"></a><span data-ttu-id="2368d-116">Vytvoření databáze pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2368d-116">Create a database with Visual Studio</span></span>
<span data-ttu-id="2368d-117">Pokud jste nový tooVisual Studio, najdete v článku hello [dotazu Azure SQL Data Warehouse (Visual Studio)][Query Azure SQL Data Warehouse (Visual Studio)].</span><span class="sxs-lookup"><span data-stu-id="2368d-117">If you are new tooVisual Studio, see hello article [Query Azure SQL Data Warehouse (Visual Studio)][Query Azure SQL Data Warehouse (Visual Studio)].</span></span>  <span data-ttu-id="2368d-118">toostart, otevřete Průzkumník objektů systému SQL Server v sadě Visual Studio a připojte toohello serveru, který bude hostitelem databáze SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="2368d-118">toostart, open SQL Server Object Explorer in Visual Studio and connect toohello server that will host your SQL Data Warehouse database.</span></span>  <span data-ttu-id="2368d-119">Po připojení můžete vytvořit SQL Data Warehouse spuštěním následujícího příkazu SQL hello hello **hlavní** databáze.</span><span class="sxs-lookup"><span data-stu-id="2368d-119">Once connected, you can create a SQL Data Warehouse by running hello following SQL command against hello **master** database.</span></span>  <span data-ttu-id="2368d-120">Tento příkaz vytvoří hello databázi MySqlDwDb s cílem služby DW400 a povolit hello databáze toogrow tooa maximální velikosti 10 TB.</span><span class="sxs-lookup"><span data-stu-id="2368d-120">This command creates hello database MySqlDwDb with a Service Objective of DW400 and allow hello database toogrow tooa maximum size of 10 TB.</span></span>

```sql
CREATE DATABASE MySqlDwDb COLLATE SQL_Latin1_General_CP1_CI_AS (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB);
```

## <a name="create-a-database-with-sqlcmd"></a><span data-ttu-id="2368d-121">Vytvoření databáze pomocí sqlcmd</span><span class="sxs-lookup"><span data-stu-id="2368d-121">Create a database with sqlcmd</span></span>
<span data-ttu-id="2368d-122">Alternativně můžete spustit stejný příkaz pomocí sqlcmd spuštěním hello následující na příkazovém řádku hello.</span><span class="sxs-lookup"><span data-stu-id="2368d-122">Alternatively, you can run hello same command with sqlcmd by running hello following at a command prompt.</span></span>

```sql
sqlcmd -S <Server Name>.database.windows.net -I -U <User> -P <Password> -Q "CREATE DATABASE MySqlDwDb COLLATE SQL_Latin1_General_CP1_CI_AS (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB)"
```

<span data-ttu-id="2368d-123">Hello výchozí kolace není-li zadána je COLLATE SQL_Latin1_General_CP1_CI_AS.</span><span class="sxs-lookup"><span data-stu-id="2368d-123">hello default collation when not specified is COLLATE SQL_Latin1_General_CP1_CI_AS.</span></span>  <span data-ttu-id="2368d-124">Hello `MAXSIZE` může být v rozmezí 250 GB do 240 TB.</span><span class="sxs-lookup"><span data-stu-id="2368d-124">hello `MAXSIZE` can be between 250 GB and 240 TB.</span></span>  <span data-ttu-id="2368d-125">Hello `SERVICE_OBJECTIVE` může být v rozmezí od DW100 do DW2000 [DWU][DWU].</span><span class="sxs-lookup"><span data-stu-id="2368d-125">hello `SERVICE_OBJECTIVE` can be between DW100 and DW2000 [DWU][DWU].</span></span>  <span data-ttu-id="2368d-126">Seznam všech platných hodnot najdete v tématu MSDN dokumentaci hello [CREATE DATABASE][CREATE DATABASE].</span><span class="sxs-lookup"><span data-stu-id="2368d-126">For a list of all valid values, see hello MSDN documentation for [CREATE DATABASE][CREATE DATABASE].</span></span>  <span data-ttu-id="2368d-127">Hello MAXSIZE a SERVICE_OBJECTIVE může být změněna pomocí [ALTER DATABASE] [ ALTER DATABASE] příkazů T-SQL.</span><span class="sxs-lookup"><span data-stu-id="2368d-127">Both hello MAXSIZE and SERVICE_OBJECTIVE can be changed with an [ALTER DATABASE][ALTER DATABASE] T-SQL command.</span></span>  <span data-ttu-id="2368d-128">Hello kolaci databáze nelze změnit po vytvoření.</span><span class="sxs-lookup"><span data-stu-id="2368d-128">hello collation of a database cannot be changed after creation.</span></span>   <span data-ttu-id="2368d-129">Upozornění: je třeba použít při změna hello SERVICE_OBJECTIVE jako změna DWU způsobí restartování služeb, která zruší všechny dotazy na cestě.</span><span class="sxs-lookup"><span data-stu-id="2368d-129">Caution should be used when changing hello SERVICE_OBJECTIVE as changing DWU causes a restart of services, which cancels all queries in flight.</span></span>  <span data-ttu-id="2368d-130">Změna hodnoty parametru MAXSIZE nerestartuje služby, protože jde pouze o operaci s metadaty.</span><span class="sxs-lookup"><span data-stu-id="2368d-130">Changing MAXSIZE does not restart services as it is just a simple metadata operation.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2368d-131">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2368d-131">Next steps</span></span>
<span data-ttu-id="2368d-132">Po dokončení zřizování, můžete svoji službu SQL Data Warehouse [načíst ukázková data] [ load sample data] nebo podívat, jak příliš[vyvíjet][develop], [načíst][load], nebo [migrovat][migrate].</span><span class="sxs-lookup"><span data-stu-id="2368d-132">After your SQL Data Warehouse has finished provisioning you can [load sample data][load sample data] or check out how too[develop][develop], [load][load], or [migrate][migrate].</span></span>

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
