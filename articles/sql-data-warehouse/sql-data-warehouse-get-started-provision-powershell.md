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
# <a name="create-sql-data-warehouse-using-powershell"></a><span data-ttu-id="817c5-103">Vytvoření SQL Data Warehouse pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="817c5-103">Create SQL Data Warehouse using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="817c5-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="817c5-104">Azure Portal</span></span>](sql-data-warehouse-get-started-provision.md)
> * [<span data-ttu-id="817c5-105">TSQL</span><span class="sxs-lookup"><span data-stu-id="817c5-105">TSQL</span></span>](sql-data-warehouse-get-started-create-database-tsql.md)
> * [<span data-ttu-id="817c5-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="817c5-106">PowerShell</span></span>](sql-data-warehouse-get-started-provision-powershell.md)
>
>

<span data-ttu-id="817c5-107">Tento článek ukazuje, jak toocreate SQL datového skladu pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="817c5-107">This article shows you how toocreate a SQL Data Warehouse using PowerShell.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="817c5-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="817c5-108">Prerequisites</span></span>
<span data-ttu-id="817c5-109">tooget spuštění, budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="817c5-109">tooget started, you need:</span></span>

* <span data-ttu-id="817c5-110">**Účet Azure**: navštivte [bezplatná zkušební verze Azure] [ Azure Free Trial] nebo [kredity Azure MSDN] [ MSDN Azure Credits] toocreate účet.</span><span class="sxs-lookup"><span data-stu-id="817c5-110">**Azure account**: Visit [Azure Free Trial][Azure Free Trial] or [MSDN Azure Credits][MSDN Azure Credits] toocreate an account.</span></span>
* <span data-ttu-id="817c5-111">**Azure SQL server**: najdete v části [vytvoření Azure SQL database v hello portálu Azure] [ Create an Azure SQL database in hello Azure Portal] nebo [vytvoření Azure SQL database pomocí prostředí PowerShell] [ Create an Azure SQL database with PowerShell] další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="817c5-111">**Azure SQL server**:  See [Create an Azure SQL database in hello Azure Portal][Create an Azure SQL database in hello Azure Portal] or [Create an Azure SQL database with PowerShell][Create an Azure SQL database with PowerShell] for more details.</span></span>
* <span data-ttu-id="817c5-112">**Skupina prostředků**: použijte hello stejný zdroj skupiny jako server Azure SQL nebo v tématu [jak toocreate skupinu prostředků](../azure-resource-manager/resource-group-portal.md).</span><span class="sxs-lookup"><span data-stu-id="817c5-112">**Resource group**: Either use hello same resource group as your Azure SQL server or see [how toocreate a resource group](../azure-resource-manager/resource-group-portal.md).</span></span>
* <span data-ttu-id="817c5-113">**Prostředí PowerShell verze 1.0.3 nebo novější:** To, jakou máte verzi, můžete zjistit spuštěním rutiny **Get-Module -ListAvailable -Name Azure**.</span><span class="sxs-lookup"><span data-stu-id="817c5-113">**PowerShell version 1.0.3 or greater**:  You can check your version by running **Get-Module -ListAvailable -Name Azure**.</span></span>  <span data-ttu-id="817c5-114">je možné nainstalovat nejnovější verzi Hello od [instalačního programu webové platformy Microsoft][Microsoft Web Platform Installer].</span><span class="sxs-lookup"><span data-stu-id="817c5-114">hello latest version can be installed from [Microsoft Web Platform Installer][Microsoft Web Platform Installer].</span></span>  <span data-ttu-id="817c5-115">Další informace o instalaci nejnovější verze hello najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell][How tooinstall and configure Azure PowerShell].</span><span class="sxs-lookup"><span data-stu-id="817c5-115">For more information on installing hello latest version, see [How tooinstall and configure Azure PowerShell][How tooinstall and configure Azure PowerShell].</span></span>

> [!NOTE]
> <span data-ttu-id="817c5-116">Vytvoření služby SQL Data Warehouse může znamenat, že se vám začne fakturovat nová služba.</span><span class="sxs-lookup"><span data-stu-id="817c5-116">Creating a SQL Data Warehouse may result in a new billable service.</span></span>  <span data-ttu-id="817c5-117">Další podrobnosti o cenách najdete v tématu [SQL Data Warehouse – ceny][SQL Data Warehouse pricing].</span><span class="sxs-lookup"><span data-stu-id="817c5-117">See [SQL Data Warehouse pricing][SQL Data Warehouse pricing] for more details on pricing.</span></span>
>
>

## <a name="create-a-sql-data-warehouse"></a><span data-ttu-id="817c5-118">Vytvoření SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="817c5-118">Create a SQL Data Warehouse</span></span>
1. <span data-ttu-id="817c5-119">Otevřete Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="817c5-119">Open Windows PowerShell.</span></span>
2. <span data-ttu-id="817c5-120">Spusťte tuto rutinu toologin tooAzure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="817c5-120">Run this cmdlet toologin tooAzure Resource Manager.</span></span>

    ```Powershell
    Login-AzureRmAccount
    ```
3. <span data-ttu-id="817c5-121">Vyberte předplatné hello chcete toouse pro aktuální relaci.</span><span class="sxs-lookup"><span data-stu-id="817c5-121">Select hello subscription you want toouse for your current session.</span></span>

    ```Powershell
    Get-AzureRmSubscription    -SubscriptionName "MySubscription" | Select-AzureRmSubscription
    ```
4. <span data-ttu-id="817c5-122">Vytvořte databázi.</span><span class="sxs-lookup"><span data-stu-id="817c5-122">Create database.</span></span> <span data-ttu-id="817c5-123">Tento příklad vytvoří databáze s názvem "mynewsqldw", s úrovní cíle služby "DW400" toohello serveru s názvem "sqldwserver1", který je ve skupině prostředků hello s názvem "mywesteuroperesgp1".</span><span class="sxs-lookup"><span data-stu-id="817c5-123">This example creates a database named "mynewsqldw", with service objective level "DW400", toohello server named "sqldwserver1", which is in hello resource group named "mywesteuroperesgp1".</span></span>

   ```Powershell
   New-AzureRmSqlDatabase -RequestedServiceObjectiveName "DW400" -DatabaseName "mynewsqldw" -ServerName "sqldwserver1" -ResourceGroupName "mywesteuroperesgp1" -Edition "DataWarehouse" -CollationName "SQL_Latin1_General_CP1_CI_AS" -MaxSizeBytes 10995116277760
   ```

<span data-ttu-id="817c5-124">Požadované parametry jsou:</span><span class="sxs-lookup"><span data-stu-id="817c5-124">Required Parameters are:</span></span>

* <span data-ttu-id="817c5-125">**RequestedServiceObjectiveName**: hello množství [DWU] [ DWU] jste požádali.</span><span class="sxs-lookup"><span data-stu-id="817c5-125">**RequestedServiceObjectiveName**: hello amount of [DWU][DWU] you are requesting.</span></span>  <span data-ttu-id="817c5-126">Podporované hodnoty: DW100, DW200, DW300, DW400, DW500, DW600, DW1000, DW1200, DW1500, DW2000, DW3000 a DW6000.</span><span class="sxs-lookup"><span data-stu-id="817c5-126">Supported values are: DW100, DW200, DW300, DW400, DW500, DW600, DW1000, DW1200, DW1500, DW2000, DW3000, and DW6000.</span></span>
* <span data-ttu-id="817c5-127">**DatabaseName**: název hello hello SQL Data Warehouse, kterou vytváříte.</span><span class="sxs-lookup"><span data-stu-id="817c5-127">**DatabaseName**: hello name of hello SQL Data Warehouse that you are creating.</span></span>
* <span data-ttu-id="817c5-128">**ServerName**: název hello hello serveru, který používáte pro vytvoření (musí být verze 12).</span><span class="sxs-lookup"><span data-stu-id="817c5-128">**ServerName**: hello name of hello server that you are using for creation (must be V12).</span></span>
* <span data-ttu-id="817c5-129">**ResourceGroupName**: Skupina prostředků, kterou používáte.</span><span class="sxs-lookup"><span data-stu-id="817c5-129">**ResourceGroupName**: Resource group you are using.</span></span>  <span data-ttu-id="817c5-130">skupiny toofind dostupných prostředků v rámci vašeho předplatného použijte rutinu Get-AzureResource.</span><span class="sxs-lookup"><span data-stu-id="817c5-130">toofind available resource groups in your subscription use Get-AzureResource.</span></span>
* <span data-ttu-id="817c5-131">**Edice**: musí být "Datového skladu" toocreate SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="817c5-131">**Edition**: Must be "DataWarehouse" toocreate a SQL Data Warehouse.</span></span>

<span data-ttu-id="817c5-132">Volitelné parametry jsou:</span><span class="sxs-lookup"><span data-stu-id="817c5-132">Optional Parameters are:</span></span>

* <span data-ttu-id="817c5-133">**%{Collationname/**: hello výchozí kolace není-li zadána, je SQL_Latin1_General_CP1_CI_AS.</span><span class="sxs-lookup"><span data-stu-id="817c5-133">**CollationName**: hello default collation if not specified is SQL_Latin1_General_CP1_CI_AS.</span></span>  <span data-ttu-id="817c5-134">Kolaci nejde pro databázi změnit.</span><span class="sxs-lookup"><span data-stu-id="817c5-134">Collation cannot be changed on a database.</span></span>
* <span data-ttu-id="817c5-135">**MaxSizeBytes**: hello výchozí maximální velikost databáze je 10 GB.</span><span class="sxs-lookup"><span data-stu-id="817c5-135">**MaxSizeBytes**: hello default max size of a database is 10 GB.</span></span>

<span data-ttu-id="817c5-136">Další podrobnosti o možnostech parametr hello najdete v tématu [New-AzureRmSqlDatabase] [ New-AzureRmSqlDatabase] a [Create Database (Azure SQL Data Warehouse)][Create Database (Azure SQL Data Warehouse)].</span><span class="sxs-lookup"><span data-stu-id="817c5-136">For more details on hello parameter options, see [New-AzureRmSqlDatabase][New-AzureRmSqlDatabase] and [Create Database (Azure SQL Data Warehouse)][Create Database (Azure SQL Data Warehouse)].</span></span>

## <a name="next-steps"></a><span data-ttu-id="817c5-137">Další kroky</span><span class="sxs-lookup"><span data-stu-id="817c5-137">Next steps</span></span>
<span data-ttu-id="817c5-138">Po dokončení SQL Data Warehouse zřizování, můžete chtít tootry [načíst ukázková data] [ loading sample data] nebo podívat, jak příliš[vyvíjet] [ develop] , [načíst][load], nebo [migrovat][migrate].</span><span class="sxs-lookup"><span data-stu-id="817c5-138">After your SQL Data Warehouse has finished provisioning you may want tootry [loading sample data][loading sample data] or check out how too[develop][develop], [load][load], or [migrate][migrate].</span></span>

<span data-ttu-id="817c5-139">Pokud vás zajímají další informace o tom toomanage SQL Data Warehouse prostřednictvím kódu programu, podívejte se na náš článek o toouse [rutiny prostředí PowerShell a rozhraní REST API][PowerShell cmdlets and REST APIs].</span><span class="sxs-lookup"><span data-stu-id="817c5-139">If you're interested in more on how toomanage SQL Data Warehouse programmatically, check out our article on how toouse [PowerShell cmdlets and REST APIs][PowerShell cmdlets and REST APIs].</span></span>

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
