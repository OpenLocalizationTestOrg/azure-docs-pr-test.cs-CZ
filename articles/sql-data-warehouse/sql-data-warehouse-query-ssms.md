---
title: aaaConnect tooAzure SQL Data Warehouse - SSMS | Microsoft Docs
description: "Pomocí SQL Server Management Studio (SSMS) tooconnect tooand dotazu Azure SQL Data Warehouse."
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
ms.openlocfilehash: bcbaf7139d2e5183b388b8d58c015cf5ad726722
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-toosql-data-warehouse-with-sql-server-management-studio-ssms"></a><span data-ttu-id="6ed11-103">Připojit tooSQL datového skladu s SQL Server Management Studio (SSMS)</span><span class="sxs-lookup"><span data-stu-id="6ed11-103">Connect tooSQL Data Warehouse with SQL Server Management Studio (SSMS)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="6ed11-104">Power BI</span><span class="sxs-lookup"><span data-stu-id="6ed11-104">Power BI</span></span>](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [<span data-ttu-id="6ed11-105">Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="6ed11-105">Azure Machine Learning</span></span>](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [<span data-ttu-id="6ed11-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6ed11-106">Visual Studio</span></span>](sql-data-warehouse-query-visual-studio.md)
> * [<span data-ttu-id="6ed11-107">sqlcmd</span><span class="sxs-lookup"><span data-stu-id="6ed11-107">sqlcmd</span></span>](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [<span data-ttu-id="6ed11-108">SSMS</span><span class="sxs-lookup"><span data-stu-id="6ed11-108">SSMS</span></span>](sql-data-warehouse-query-ssms.md)
> 
> 

<span data-ttu-id="6ed11-109">Pomocí SQL Server Management Studio (SSMS) tooconnect tooand dotazu Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="6ed11-109">Use SQL Server Management Studio (SSMS) tooconnect tooand query Azure SQL Data Warehouse.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="6ed11-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6ed11-110">Prerequisites</span></span>
<span data-ttu-id="6ed11-111">toouse tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="6ed11-111">toouse this tutorial, you need:</span></span>

* <span data-ttu-id="6ed11-112">Existující SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="6ed11-112">An existing SQL data warehouse.</span></span> <span data-ttu-id="6ed11-113">toocreate, najdete v části [vytvořit SQL Data Warehouse][Create a SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="6ed11-113">toocreate one, see [Create a SQL Data Warehouse][Create a SQL Data Warehouse].</span></span>
* <span data-ttu-id="6ed11-114">SQL Server Management Studio (SSMS) nainstalována.</span><span class="sxs-lookup"><span data-stu-id="6ed11-114">SQL Server Management Studio (SSMS) installed.</span></span> <span data-ttu-id="6ed11-115">[Instalace aplikace SSMS] [ Install SSMS] zdarma, pokud ještě nemáte ho.</span><span class="sxs-lookup"><span data-stu-id="6ed11-115">[Install SSMS][Install SSMS] for free if you don't already have it.</span></span>
* <span data-ttu-id="6ed11-116">Hello plně kvalifikovaný název serveru SQL server.</span><span class="sxs-lookup"><span data-stu-id="6ed11-116">hello fully qualified SQL server name.</span></span> <span data-ttu-id="6ed11-117">toofind, najdete v tématu [připojit tooSQL datového skladu][Connect tooSQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="6ed11-117">toofind this, see [Connect tooSQL Data Warehouse][Connect tooSQL Data Warehouse].</span></span>

## <a name="1-connect-tooyour-sql-data-warehouse"></a><span data-ttu-id="6ed11-118">1. Připojit tooyour SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="6ed11-118">1. Connect tooyour SQL Data Warehouse</span></span>
1. <span data-ttu-id="6ed11-119">Otevřete aplikaci SSMS.</span><span class="sxs-lookup"><span data-stu-id="6ed11-119">Open SSMS.</span></span>
2. <span data-ttu-id="6ed11-120">Otevřete Průzkumník objektů.</span><span class="sxs-lookup"><span data-stu-id="6ed11-120">Open Object Explorer.</span></span> <span data-ttu-id="6ed11-121">toodo se vyberte **soubor** > **připojit Průzkumník objektů**.</span><span class="sxs-lookup"><span data-stu-id="6ed11-121">toodo this, select **File** > **Connect Object Explorer**.</span></span>
   
    ![Průzkumník objektů systému SQL Server][1]
3. <span data-ttu-id="6ed11-123">Vyplňte pole hello v okně tooServer hello připojit.</span><span class="sxs-lookup"><span data-stu-id="6ed11-123">Fill in hello fields in hello Connect tooServer window.</span></span>
   
    ![Připojit tooServer][2]
   
   * <span data-ttu-id="6ed11-125">**Název serveru**:</span><span class="sxs-lookup"><span data-stu-id="6ed11-125">**Server name**.</span></span> <span data-ttu-id="6ed11-126">Zadejte hello **název serveru** dříve identifikovat.</span><span class="sxs-lookup"><span data-stu-id="6ed11-126">Enter hello **server name** previously identified.</span></span>
   * <span data-ttu-id="6ed11-127">**Ověřování**:</span><span class="sxs-lookup"><span data-stu-id="6ed11-127">**Authentication**.</span></span> <span data-ttu-id="6ed11-128">Vyberte **Ověřování serveru SQL Server** nebo **Integrované ověřování Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="6ed11-128">Select **SQL Server Authentication** or **Active Directory Integrated Authentication**.</span></span>
   * <span data-ttu-id="6ed11-129">**Uživatelské jméno** a **Heslo**:</span><span class="sxs-lookup"><span data-stu-id="6ed11-129">**User Name** and **Password**.</span></span> <span data-ttu-id="6ed11-130">Pokud jste výše vybrali možnost Ověřování serveru SQL Server, zadejte uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="6ed11-130">Enter user name and password if SQL Server Authentication was selected above.</span></span>
   * <span data-ttu-id="6ed11-131">Klikněte na **Připojit**.</span><span class="sxs-lookup"><span data-stu-id="6ed11-131">Click **Connect**.</span></span>
4. <span data-ttu-id="6ed11-132">tooexplore, rozbalte položku serveru Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="6ed11-132">tooexplore, expand your Azure SQL server.</span></span> <span data-ttu-id="6ed11-133">Můžete zobrazit hello databáze přidružené k serveru hello.</span><span class="sxs-lookup"><span data-stu-id="6ed11-133">You can view hello databases associated with hello server.</span></span> <span data-ttu-id="6ed11-134">Rozbalte položku AdventureWorksDW toosee hello tabulky v ukázkové databázi.</span><span class="sxs-lookup"><span data-stu-id="6ed11-134">Expand AdventureWorksDW toosee hello tables in your sample database.</span></span>
   
    ![Prozkoumejte AdventureWorksDW.][3]

## <a name="2-run-a-sample-query"></a><span data-ttu-id="6ed11-136">2. Spuštění ukázkového dotazu</span><span class="sxs-lookup"><span data-stu-id="6ed11-136">2. Run a sample query</span></span>
<span data-ttu-id="6ed11-137">Teď, když připojení bylo navázáno tooyour databáze, můžete napsat dotaz.</span><span class="sxs-lookup"><span data-stu-id="6ed11-137">Now that a connection has been established tooyour database, let's write a query.</span></span>

1. <span data-ttu-id="6ed11-138">Pravým tlačítkem myši klikněte na databázi v Průzkumníku objektů systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="6ed11-138">Right-click your database in SQL Server Object Explorer.</span></span>
2. <span data-ttu-id="6ed11-139">Vyberte **Nový dotaz**.</span><span class="sxs-lookup"><span data-stu-id="6ed11-139">Select **New Query**.</span></span> <span data-ttu-id="6ed11-140">Otevře se nové okno dotazu.</span><span class="sxs-lookup"><span data-stu-id="6ed11-140">A new query window opens.</span></span>
   
    ![Nový dotaz][4]
3. <span data-ttu-id="6ed11-142">Zkopírujte tento dotaz TSQL do okna dotazu hello:</span><span class="sxs-lookup"><span data-stu-id="6ed11-142">Copy this TSQL query into hello query window:</span></span>
   
    ```sql
    SELECT COUNT(*) FROM dbo.FactInternetSales;
    ```
4. <span data-ttu-id="6ed11-143">Spusťte dotaz hello.</span><span class="sxs-lookup"><span data-stu-id="6ed11-143">Run hello query.</span></span> <span data-ttu-id="6ed11-144">toodo tento, klikněte na tlačítko `Execute` nebo hello použijte následující klávesové: `F5`.</span><span class="sxs-lookup"><span data-stu-id="6ed11-144">toodo this, click `Execute` or use hello following shortcut: `F5`.</span></span>
   
    ![Spuštění dotazu][5]
5. <span data-ttu-id="6ed11-146">Podívejte se na výsledky dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="6ed11-146">Look at hello query results.</span></span> <span data-ttu-id="6ed11-147">V tomto příkladu má hello tabulka FactInternetSales 60 398 řádků.</span><span class="sxs-lookup"><span data-stu-id="6ed11-147">In this example, hello FactInternetSales table has 60398 rows.</span></span>
   
    ![Výsledky dotazu][6]

## <a name="next-steps"></a><span data-ttu-id="6ed11-149">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6ed11-149">Next steps</span></span>
<span data-ttu-id="6ed11-150">Teď, když se můžete připojit a dotazu, zkuste [vizualizace hello dat pomocí PowerBI][visualizing hello data with PowerBI].</span><span class="sxs-lookup"><span data-stu-id="6ed11-150">Now that you can connect and query, try [visualizing hello data with PowerBI][visualizing hello data with PowerBI].</span></span>

<span data-ttu-id="6ed11-151">tooconfigure prostředí pro ověřování Azure Active Directory, najdete v části [ověření tooSQL datového skladu][Authenticate tooSQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="6ed11-151">tooconfigure your environment for Azure Active Directory authentication, see [Authenticate tooSQL Data Warehouse][Authenticate tooSQL Data Warehouse].</span></span>

<!--Arcticles-->
[Connect tooSQL Data Warehouse]: sql-data-warehouse-connect-overview.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Authenticate tooSQL Data Warehouse]: sql-data-warehouse-authentication.md
[visualizing hello data with PowerBI]: sql-data-warehouse-get-started-visualize-with-power-bi.md 

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
