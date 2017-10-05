---
title: "Připojení k Azure SQL Data Warehouse - SSMS | Microsoft Docs"
description: "SQL Server Management Studio (SSMS) použijte k připojení a dotazování Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: 
author: antvgski
manager: jhubbard
editor: 
ms.assetid: 299e50b3-e68a-471c-8aee-b0b9874781bd
ms.service: sql-data-warehouse
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.custom: connect
ms.date: 10/31/2016
ms.author: anvang;barbkess
ms.openlocfilehash: 207fb9fd861c66039fbde89681aed3df3a2f4021
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="connect-to-sql-data-warehouse-with-sql-server-management-studio-ssms"></a><span data-ttu-id="37698-103">Připojení k SQL Data Warehouse pomocí SQL Server Management Studio (SSMS)</span><span class="sxs-lookup"><span data-stu-id="37698-103">Connect to SQL Data Warehouse with SQL Server Management Studio (SSMS)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="37698-104">Power BI</span><span class="sxs-lookup"><span data-stu-id="37698-104">Power BI</span></span>](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [<span data-ttu-id="37698-105">Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="37698-105">Azure Machine Learning</span></span>](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [<span data-ttu-id="37698-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="37698-106">Visual Studio</span></span>](sql-data-warehouse-query-visual-studio.md)
> * [<span data-ttu-id="37698-107">sqlcmd</span><span class="sxs-lookup"><span data-stu-id="37698-107">sqlcmd</span></span>](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [<span data-ttu-id="37698-108">SSMS</span><span class="sxs-lookup"><span data-stu-id="37698-108">SSMS</span></span>](sql-data-warehouse-query-ssms.md)
> 
> 

<span data-ttu-id="37698-109">SQL Server Management Studio (SSMS) použijte k připojení a dotazování Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="37698-109">Use SQL Server Management Studio (SSMS) to connect to and query Azure SQL Data Warehouse.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="37698-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="37698-110">Prerequisites</span></span>
<span data-ttu-id="37698-111">Chcete-li použít tento kurz, potřebujete:</span><span class="sxs-lookup"><span data-stu-id="37698-111">To use this tutorial, you need:</span></span>

* <span data-ttu-id="37698-112">Existující SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="37698-112">An existing SQL data warehouse.</span></span> <span data-ttu-id="37698-113">Pokud chcete jeden vytvořit, podívejte se na téma [Vytvoření SQL Data Warehouse][Create a SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="37698-113">To create one, see [Create a SQL Data Warehouse][Create a SQL Data Warehouse].</span></span>
* <span data-ttu-id="37698-114">SQL Server Management Studio (SSMS) nainstalována.</span><span class="sxs-lookup"><span data-stu-id="37698-114">SQL Server Management Studio (SSMS) installed.</span></span> <span data-ttu-id="37698-115">[Instalace aplikace SSMS] [ Install SSMS] zdarma, pokud ještě nemáte ho.</span><span class="sxs-lookup"><span data-stu-id="37698-115">[Install SSMS][Install SSMS] for free if you don't already have it.</span></span>
* <span data-ttu-id="37698-116">Plně kvalifikovaný název serveru SQL.</span><span class="sxs-lookup"><span data-stu-id="37698-116">The fully qualified SQL server name.</span></span> <span data-ttu-id="37698-117">Ten zjistíte v části [Připojení k SQL Data Warehouse][Connect to SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="37698-117">To find this, see [Connect to SQL Data Warehouse][Connect to SQL Data Warehouse].</span></span>

## <a name="1-connect-to-your-sql-data-warehouse"></a><span data-ttu-id="37698-118">1. Připojení k vaší službě SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="37698-118">1. Connect to your SQL Data Warehouse</span></span>
1. <span data-ttu-id="37698-119">Otevřete aplikaci SSMS.</span><span class="sxs-lookup"><span data-stu-id="37698-119">Open SSMS.</span></span>
2. <span data-ttu-id="37698-120">Otevřete Průzkumník objektů.</span><span class="sxs-lookup"><span data-stu-id="37698-120">Open Object Explorer.</span></span> <span data-ttu-id="37698-121">Chcete-li to provést, vyberte **soubor** > **připojit Průzkumník objektů**.</span><span class="sxs-lookup"><span data-stu-id="37698-121">To do this, select **File** > **Connect Object Explorer**.</span></span>
   
    ![Průzkumník objektů systému SQL Server][1]
3. <span data-ttu-id="37698-123">Vyplňte pole v okně pro připojení k serveru.</span><span class="sxs-lookup"><span data-stu-id="37698-123">Fill in the fields in the Connect to Server window.</span></span>
   
    ![Připojení k serveru][2]
   
   * <span data-ttu-id="37698-125">**Název serveru**:</span><span class="sxs-lookup"><span data-stu-id="37698-125">**Server name**.</span></span> <span data-ttu-id="37698-126">Zadejte **název serveru**, který jste si zjistili.</span><span class="sxs-lookup"><span data-stu-id="37698-126">Enter the **server name** previously identified.</span></span>
   * <span data-ttu-id="37698-127">**Ověřování**:</span><span class="sxs-lookup"><span data-stu-id="37698-127">**Authentication**.</span></span> <span data-ttu-id="37698-128">Vyberte **Ověřování serveru SQL Server** nebo **Integrované ověřování Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="37698-128">Select **SQL Server Authentication** or **Active Directory Integrated Authentication**.</span></span>
   * <span data-ttu-id="37698-129">**Uživatelské jméno** a **Heslo**:</span><span class="sxs-lookup"><span data-stu-id="37698-129">**User Name** and **Password**.</span></span> <span data-ttu-id="37698-130">Pokud jste výše vybrali možnost Ověřování serveru SQL Server, zadejte uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="37698-130">Enter user name and password if SQL Server Authentication was selected above.</span></span>
   * <span data-ttu-id="37698-131">Klikněte na **Připojit**.</span><span class="sxs-lookup"><span data-stu-id="37698-131">Click **Connect**.</span></span>
4. <span data-ttu-id="37698-132">Pokud chcete SQL server Azure prozkoumat, rozbalte ho.</span><span class="sxs-lookup"><span data-stu-id="37698-132">To explore, expand your Azure SQL server.</span></span> <span data-ttu-id="37698-133">Můžete se podívat, které databáze jsou k tomuto serveru přidružené.</span><span class="sxs-lookup"><span data-stu-id="37698-133">You can view the databases associated with the server.</span></span> <span data-ttu-id="37698-134">Rozbalte položku AdventureWorksDW a podívejte se na tabulky v ukázkové databázi.</span><span class="sxs-lookup"><span data-stu-id="37698-134">Expand AdventureWorksDW to see the tables in your sample database.</span></span>
   
    ![Prozkoumejte AdventureWorksDW.][3]

## <a name="2-run-a-sample-query"></a><span data-ttu-id="37698-136">2. Spuštění ukázkového dotazu</span><span class="sxs-lookup"><span data-stu-id="37698-136">2. Run a sample query</span></span>
<span data-ttu-id="37698-137">Teď, když jste si vytvořili připojení k databázi, můžete napsat dotaz.</span><span class="sxs-lookup"><span data-stu-id="37698-137">Now that a connection has been established to your database, let's write a query.</span></span>

1. <span data-ttu-id="37698-138">Pravým tlačítkem myši klikněte na databázi v Průzkumníku objektů systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="37698-138">Right-click your database in SQL Server Object Explorer.</span></span>
2. <span data-ttu-id="37698-139">Vyberte **Nový dotaz**.</span><span class="sxs-lookup"><span data-stu-id="37698-139">Select **New Query**.</span></span> <span data-ttu-id="37698-140">Otevře se nové okno dotazu.</span><span class="sxs-lookup"><span data-stu-id="37698-140">A new query window opens.</span></span>
   
    ![Nový dotaz][4]
3. <span data-ttu-id="37698-142">Zkopírujte tento dotaz TSQL do okna dotazu:</span><span class="sxs-lookup"><span data-stu-id="37698-142">Copy this TSQL query into the query window:</span></span>
   
    ```sql
    SELECT COUNT(*) FROM dbo.FactInternetSales;
    ```
4. <span data-ttu-id="37698-143">Spusťte dotaz.</span><span class="sxs-lookup"><span data-stu-id="37698-143">Run the query.</span></span> <span data-ttu-id="37698-144">Chcete-li to provést, klikněte na tlačítko `Execute` nebo použijte následující klávesovou zkratku: `F5`.</span><span class="sxs-lookup"><span data-stu-id="37698-144">To do this, click `Execute` or use the following shortcut: `F5`.</span></span>
   
    ![Spuštění dotazu][5]
5. <span data-ttu-id="37698-146">Podívejte se na výsledky dotazu.</span><span class="sxs-lookup"><span data-stu-id="37698-146">Look at the query results.</span></span> <span data-ttu-id="37698-147">V tomto příkladě má tabulka FactInternetSales 60 398 řádků.</span><span class="sxs-lookup"><span data-stu-id="37698-147">In this example, the FactInternetSales table has 60398 rows.</span></span>
   
    ![Výsledky dotazu][6]

## <a name="next-steps"></a><span data-ttu-id="37698-149">Další kroky</span><span class="sxs-lookup"><span data-stu-id="37698-149">Next steps</span></span>
<span data-ttu-id="37698-150">Teď, když se můžete připojit a můžete zadávat dotazy, můžete si vyzkoušet [vizualizaci dat pomocí PowerBI][visualizing the data with PowerBI].</span><span class="sxs-lookup"><span data-stu-id="37698-150">Now that you can connect and query, try [visualizing the data with PowerBI][visualizing the data with PowerBI].</span></span>

<span data-ttu-id="37698-151">Informace o tom, jak nakonfigurovat prostředí pro ověřování služby Azure Active Directory, najdete v části [Ověřování pro SQL Data Warehouse][Authenticate to SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="37698-151">To configure your environment for Azure Active Directory authentication, see [Authenticate to SQL Data Warehouse][Authenticate to SQL Data Warehouse].</span></span>

<!--Arcticles-->
[Connect to SQL Data Warehouse]: sql-data-warehouse-connect-overview.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Authenticate to SQL Data Warehouse]: sql-data-warehouse-authentication.md
[visualizing the data with PowerBI]: sql-data-warehouse-get-started-visualize-with-power-bi.md 

<!--Other-->
[Azure portal]: https://portal.azure.com
[Install SSMS]: https://msdn.microsoft.com/en-US/library/hh213248.aspx


<!--Image references-->

[1]: media/sql-data-warehouse-query-ssms/connect-object-explorer.png
[2]: media/sql-data-warehouse-query-ssms/connect-object-explorer1.png
[3]: media/sql-data-warehouse-query-ssms/explore-tables.png
[4]: media/sql-data-warehouse-query-ssms/new-query.png
[5]: media/sql-data-warehouse-query-ssms/execute-query.png
[6]: media/sql-data-warehouse-query-ssms/results.png
