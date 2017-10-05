---
title: "Vytvoření SQL Data Warehouse pomocí prostředí PowerShell | Dokumentace Microsoftu"
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
ms.openlocfilehash: a763f1c600c1a3f37cb565a8eb7db3c3f27dcf75
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-sql-data-warehouse-using-powershell"></a><span data-ttu-id="c9390-103">Vytvoření SQL Data Warehouse pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="c9390-103">Create SQL Data Warehouse using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="c9390-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c9390-104">Azure Portal</span></span>](sql-data-warehouse-get-started-provision.md)
> * [<span data-ttu-id="c9390-105">TSQL</span><span class="sxs-lookup"><span data-stu-id="c9390-105">TSQL</span></span>](sql-data-warehouse-get-started-create-database-tsql.md)
> * [<span data-ttu-id="c9390-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="c9390-106">PowerShell</span></span>](sql-data-warehouse-get-started-provision-powershell.md)
>
>

<span data-ttu-id="c9390-107">V tomto článku zjistíte, jak můžete k vytvoření SQL Data Warehouse použít PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c9390-107">This article shows you how to create a SQL Data Warehouse using PowerShell.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c9390-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c9390-108">Prerequisites</span></span>
<span data-ttu-id="c9390-109">Na začátek budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="c9390-109">To get started, you need:</span></span>

* <span data-ttu-id="c9390-110">**Účet Azure**: Pokud si chcete vytvořit účet, přejděte na stránku [Bezplatná zkušební verze Azure][Azure Free Trial] nebo [Kredity Azure pro předplatitele MSDN][MSDN Azure Credits].</span><span class="sxs-lookup"><span data-stu-id="c9390-110">**Azure account**: Visit [Azure Free Trial][Azure Free Trial] or [MSDN Azure Credits][MSDN Azure Credits] to create an account.</span></span>
* <span data-ttu-id="c9390-111">**Azure SQL Server:** Přečtěte si článek [Vytvoření Azure SQL Database pomocí webu Azure Portal][Create an Azure SQL database in the Azure Portal] nebo [Vytvoření Azure SQL Database pomocí prostředí PowerShell][Create an Azure SQL database with PowerShell], kde najdete další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="c9390-111">**Azure SQL server**:  See [Create an Azure SQL database in the Azure Portal][Create an Azure SQL database in the Azure Portal] or [Create an Azure SQL database with PowerShell][Create an Azure SQL database with PowerShell] for more details.</span></span>
* <span data-ttu-id="c9390-112">**Skupinu prostředků:** Buď použijte stejnou skupinu prostředků jako pro Azure SQL Server, nebo zjistěte, [jak vytvořit skupinu prostředků](../azure-resource-manager/resource-group-portal.md).</span><span class="sxs-lookup"><span data-stu-id="c9390-112">**Resource group**: Either use the same resource group as your Azure SQL server or see [how to create a resource group](../azure-resource-manager/resource-group-portal.md).</span></span>
* <span data-ttu-id="c9390-113">**Prostředí PowerShell verze 1.0.3 nebo novější:** To, jakou máte verzi, můžete zjistit spuštěním rutiny **Get-Module -ListAvailable -Name Azure**.</span><span class="sxs-lookup"><span data-stu-id="c9390-113">**PowerShell version 1.0.3 or greater**:  You can check your version by running **Get-Module -ListAvailable -Name Azure**.</span></span>  <span data-ttu-id="c9390-114">Nejnovější verzi si můžete nainstalovat pomocí [instalačního programu Webové platformy Microsoft][Microsoft Web Platform Installer].</span><span class="sxs-lookup"><span data-stu-id="c9390-114">The latest version can be installed from [Microsoft Web Platform Installer][Microsoft Web Platform Installer].</span></span>  <span data-ttu-id="c9390-115">Další informace o instalaci nejnovější verze najdete v tématu [Jak nainstalovat a nakonfigurovat Azure PowerShell][How to install and configure Azure PowerShell].</span><span class="sxs-lookup"><span data-stu-id="c9390-115">For more information on installing the latest version, see [How to install and configure Azure PowerShell][How to install and configure Azure PowerShell].</span></span>

> [!NOTE]
> <span data-ttu-id="c9390-116">Vytvoření služby SQL Data Warehouse může znamenat, že se vám začne fakturovat nová služba.</span><span class="sxs-lookup"><span data-stu-id="c9390-116">Creating a SQL Data Warehouse may result in a new billable service.</span></span>  <span data-ttu-id="c9390-117">Další podrobnosti o cenách najdete v tématu [SQL Data Warehouse – ceny][SQL Data Warehouse pricing].</span><span class="sxs-lookup"><span data-stu-id="c9390-117">See [SQL Data Warehouse pricing][SQL Data Warehouse pricing] for more details on pricing.</span></span>
>
>

## <a name="create-a-sql-data-warehouse"></a><span data-ttu-id="c9390-118">Vytvoření SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="c9390-118">Create a SQL Data Warehouse</span></span>
1. <span data-ttu-id="c9390-119">Otevřete Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c9390-119">Open Windows PowerShell.</span></span>
2. <span data-ttu-id="c9390-120">Spuštěním této rutiny se přihlaste do Azure Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="c9390-120">Run this cmdlet to login to Azure Resource Manager.</span></span>

    ```Powershell
    Login-AzureRmAccount
    ```
3. <span data-ttu-id="c9390-121">Vyberte předplatné, které chcete použít pro aktuální relaci.</span><span class="sxs-lookup"><span data-stu-id="c9390-121">Select the subscription you want to use for your current session.</span></span>

    ```Powershell
    Get-AzureRmSubscription    -SubscriptionName "MySubscription" | Select-AzureRmSubscription
    ```
4. <span data-ttu-id="c9390-122">Vytvořte databázi.</span><span class="sxs-lookup"><span data-stu-id="c9390-122">Create database.</span></span> <span data-ttu-id="c9390-123">Tento příklad vytvoří databázi s názvem mynewsqldw s úrovní cíle služby DW400 na serveru s názvem sqldwserver1, který je ve skupině prostředků s názvem mywesteuroperesgp1.</span><span class="sxs-lookup"><span data-stu-id="c9390-123">This example creates a database named "mynewsqldw", with service objective level "DW400", to the server named "sqldwserver1", which is in the resource group named "mywesteuroperesgp1".</span></span>

   ```Powershell
   New-AzureRmSqlDatabase -RequestedServiceObjectiveName "DW400" -DatabaseName "mynewsqldw" -ServerName "sqldwserver1" -ResourceGroupName "mywesteuroperesgp1" -Edition "DataWarehouse" -CollationName "SQL_Latin1_General_CP1_CI_AS" -MaxSizeBytes 10995116277760
   ```

<span data-ttu-id="c9390-124">Požadované parametry jsou:</span><span class="sxs-lookup"><span data-stu-id="c9390-124">Required Parameters are:</span></span>

* <span data-ttu-id="c9390-125">**RequestedServiceObjectiveName**: Počet jednotek [DWU][DWU], o které žádáte.</span><span class="sxs-lookup"><span data-stu-id="c9390-125">**RequestedServiceObjectiveName**: The amount of [DWU][DWU] you are requesting.</span></span>  <span data-ttu-id="c9390-126">Podporované hodnoty: DW100, DW200, DW300, DW400, DW500, DW600, DW1000, DW1200, DW1500, DW2000, DW3000 a DW6000.</span><span class="sxs-lookup"><span data-stu-id="c9390-126">Supported values are: DW100, DW200, DW300, DW400, DW500, DW600, DW1000, DW1200, DW1500, DW2000, DW3000, and DW6000.</span></span>
* <span data-ttu-id="c9390-127">**DatabaseName**: Název služby SQL Data Warehouse, kterou vytváříte.</span><span class="sxs-lookup"><span data-stu-id="c9390-127">**DatabaseName**: The name of the SQL Data Warehouse that you are creating.</span></span>
* <span data-ttu-id="c9390-128">**ServerName**: Název serveru, který používáte pro vytvoření (musí to být server V12).</span><span class="sxs-lookup"><span data-stu-id="c9390-128">**ServerName**: The name of the server that you are using for creation (must be V12).</span></span>
* <span data-ttu-id="c9390-129">**ResourceGroupName**: Skupina prostředků, kterou používáte.</span><span class="sxs-lookup"><span data-stu-id="c9390-129">**ResourceGroupName**: Resource group you are using.</span></span>  <span data-ttu-id="c9390-130">K vyhledání dostupných skupin prostředků v rámci vašeho předplatného použijte rutinu Get-AzureResource.</span><span class="sxs-lookup"><span data-stu-id="c9390-130">To find available resource groups in your subscription use Get-AzureResource.</span></span>
* <span data-ttu-id="c9390-131">**Edition:** Aby bylo možné vytvořit SQL Data Warehouse, je nutné nastavit edici DataWarehouse.</span><span class="sxs-lookup"><span data-stu-id="c9390-131">**Edition**: Must be "DataWarehouse" to create a SQL Data Warehouse.</span></span>

<span data-ttu-id="c9390-132">Volitelné parametry jsou:</span><span class="sxs-lookup"><span data-stu-id="c9390-132">Optional Parameters are:</span></span>

* <span data-ttu-id="c9390-133">**CollationName:** Pokud není uvedeno, je výchozí kolace SQL_Latin1_General_CP1_CI_AS.</span><span class="sxs-lookup"><span data-stu-id="c9390-133">**CollationName**: The default collation if not specified is SQL_Latin1_General_CP1_CI_AS.</span></span>  <span data-ttu-id="c9390-134">Kolaci nejde pro databázi změnit.</span><span class="sxs-lookup"><span data-stu-id="c9390-134">Collation cannot be changed on a database.</span></span>
* <span data-ttu-id="c9390-135">**MaxSizeBytes:** Výchozí maximální velikost databáze je 10 GB.</span><span class="sxs-lookup"><span data-stu-id="c9390-135">**MaxSizeBytes**: The default max size of a database is 10 GB.</span></span>

<span data-ttu-id="c9390-136">Další podrobnosti o možných parametrech najdete v tématech [New-AzureRmSqlDatabase][New-AzureRmSqlDatabase] a [Vytvoření databáze (Azure SQL Data Warehouse)][Create Database (Azure SQL Data Warehouse)].</span><span class="sxs-lookup"><span data-stu-id="c9390-136">For more details on the parameter options, see [New-AzureRmSqlDatabase][New-AzureRmSqlDatabase] and [Create Database (Azure SQL Data Warehouse)][Create Database (Azure SQL Data Warehouse)].</span></span>

## <a name="next-steps"></a><span data-ttu-id="c9390-137">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c9390-137">Next steps</span></span>
<span data-ttu-id="c9390-138">Až se vám zřídí SQL Data Warehouse, můžete zkusit [načíst ukázková data][loading sample data] nebo se můžete podívat, jak na [vývoj][develop], [načítání][load] nebo [migraci][migrate].</span><span class="sxs-lookup"><span data-stu-id="c9390-138">After your SQL Data Warehouse has finished provisioning you may want to try [loading sample data][loading sample data] or check out how to [develop][develop], [load][load], or [migrate][migrate].</span></span>

<span data-ttu-id="c9390-139">Pokud vás zajímají další informace o tom, jak SQL Data Warehouse spravovat prostřednictvím kódu programu, podívejte se na náš článek věnovaný tomu, jak používat [rutiny prostředí PowerShell a rozhraní REST API][PowerShell cmdlets and REST APIs].</span><span class="sxs-lookup"><span data-stu-id="c9390-139">If you're interested in more on how to manage SQL Data Warehouse programmatically, check out our article on how to use [PowerShell cmdlets and REST APIs][PowerShell cmdlets and REST APIs].</span></span>

<!--Image references-->

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md
[migrate]: ./sql-data-warehouse-overview-migrate.md
[develop]: ./sql-data-warehouse-overview-develop.md
[load]: ./sql-data-warehouse-load-with-bcp.md
[loading sample data]: ./sql-data-warehouse-load-sample-databases.md
[PowerShell cmdlets and REST APIs]: ./sql-data-warehouse-reference-powershell-cmdlets.md
[firewall rules]: ../sql-database-configure-firewall-settings.md

[How to install and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
[how to create a SQL Data Warehouse from the Azure Portal]: ./sql-data-warehouse-get-started-provision.md
[Create an Azure SQL database in the Azure Portal]: ../sql-database/sql-database-get-started.md
[Create an Azure SQL database with PowerShell]: ../sql-database/sql-database-get-started-powershell.md
[how to create a resource group]: ../azure-resource-manager/resource-group-template-deploy-portal.md#create-resource-group

<!--MSDN references-->
[MSDN]: https://msdn.microsoft.com/library/azure/dn546722.aspx
[New-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619339.aspx
[Create Database (Azure SQL Data Warehouse)]: https://msdn.microsoft.com/library/mt204021.aspx

<!--Other Web references-->
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
[SQL Data Warehouse pricing]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure Free Trial]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN Azure Credits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F
