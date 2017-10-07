---
title: "aaaConnect tooAzure SQL Data Warehouse - služby VSTS | Microsoft Docs"
description: "Dotazování SQL Data Warehouse pomocí sady Visual Studio."
services: sql-data-warehouse
documentationcenter: NA
author: antvgski
manager: jhubbard
editor: 
ms.assetid: daace889-95e5-4826-b2fc-047eac9d6d95
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: connect
ms.date: 10/31/2016
ms.author: anvang;barbkess
ms.openlocfilehash: 55eef4dff3e0647be5a735295bc89b43eb456079
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-toosql-data-warehouse-with-visual-studio-and-ssdt"></a><span data-ttu-id="5c6ee-103">Připojení k sadě Visual Studio a rozšíření SSDT tooSQL datového skladu</span><span class="sxs-lookup"><span data-stu-id="5c6ee-103">Connect tooSQL Data Warehouse with Visual Studio and SSDT</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="5c6ee-104">Power BI</span><span class="sxs-lookup"><span data-stu-id="5c6ee-104">Power BI</span></span>](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [<span data-ttu-id="5c6ee-105">Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="5c6ee-105">Azure Machine Learning</span></span>](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [<span data-ttu-id="5c6ee-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5c6ee-106">Visual Studio</span></span>](sql-data-warehouse-query-visual-studio.md)
> * [<span data-ttu-id="5c6ee-107">sqlcmd</span><span class="sxs-lookup"><span data-stu-id="5c6ee-107">sqlcmd</span></span>](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [<span data-ttu-id="5c6ee-108">SSMS</span><span class="sxs-lookup"><span data-stu-id="5c6ee-108">SSMS</span></span>](sql-data-warehouse-query-ssms.md)
> 
> 

<span data-ttu-id="5c6ee-109">Pomocí sady Visual Studio tooquery Azure SQL Data Warehouse za několik minut.</span><span class="sxs-lookup"><span data-stu-id="5c6ee-109">Use Visual Studio tooquery Azure SQL Data Warehouse in just a few minutes.</span></span> <span data-ttu-id="5c6ee-110">Tato metoda používá hello rozšíření SQL Server Data Tools (SSDT) v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5c6ee-110">This method uses hello SQL Server Data Tools (SSDT) extension in Visual Studio.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="5c6ee-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5c6ee-111">Prerequisites</span></span>
<span data-ttu-id="5c6ee-112">toouse tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="5c6ee-112">toouse this tutorial, you need:</span></span>

* <span data-ttu-id="5c6ee-113">Existující SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="5c6ee-113">An existing SQL data warehouse.</span></span> <span data-ttu-id="5c6ee-114">toocreate, najdete v části [vytvořit SQL Data Warehouse][Create a SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="5c6ee-114">toocreate one, see [Create a SQL Data Warehouse][Create a SQL Data Warehouse].</span></span>
* <span data-ttu-id="5c6ee-115">SSDT pro Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5c6ee-115">SSDT for Visual Studio.</span></span> <span data-ttu-id="5c6ee-116">Pokud máte aplikaci Visual Studio, pravděpodobně již databázi máte.</span><span class="sxs-lookup"><span data-stu-id="5c6ee-116">If you have Visual Studio, you probably already have this.</span></span> <span data-ttu-id="5c6ee-117">Pokyny k instalaci a možnosti najdete v tématu věnovaném [instalaci sady Visual Studio a rozšíření SSDT][Installing Visual Studio and SSDT].</span><span class="sxs-lookup"><span data-stu-id="5c6ee-117">For installation instructions and options, see [Installing Visual Studio and SSDT][Installing Visual Studio and SSDT].</span></span>
* <span data-ttu-id="5c6ee-118">Hello plně kvalifikovaný název serveru SQL server.</span><span class="sxs-lookup"><span data-stu-id="5c6ee-118">hello fully qualified SQL server name.</span></span> <span data-ttu-id="5c6ee-119">toofind, najdete v tématu [připojit tooSQL datového skladu][Connect tooSQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="5c6ee-119">toofind this, see [Connect tooSQL Data Warehouse][Connect tooSQL Data Warehouse].</span></span>

## <a name="1-connect-tooyour-sql-data-warehouse"></a><span data-ttu-id="5c6ee-120">1. Připojit tooyour SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="5c6ee-120">1. Connect tooyour SQL Data Warehouse</span></span>
1. <span data-ttu-id="5c6ee-121">Otevřete Visual Studio 2013 nebo 2015.</span><span class="sxs-lookup"><span data-stu-id="5c6ee-121">Open Visual Studio 2013 or 2015.</span></span>
2. <span data-ttu-id="5c6ee-122">Otevřete Průzkumník objektů SQL Serveru.</span><span class="sxs-lookup"><span data-stu-id="5c6ee-122">Open SQL Server Object Explorer.</span></span> <span data-ttu-id="5c6ee-123">toodo se vyberte **zobrazení** > **Průzkumník objektů systému SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="5c6ee-123">toodo this, select **View** > **SQL Server Object Explorer**.</span></span>
   
    ![Průzkumník objektů systému SQL Server][1]
3. <span data-ttu-id="5c6ee-125">Klikněte na tlačítko hello **přidat SQL Server** ikonu.</span><span class="sxs-lookup"><span data-stu-id="5c6ee-125">Click hello **Add SQL Server** icon.</span></span>
   
    ![Přidat SQL Server][2]
4. <span data-ttu-id="5c6ee-127">Vyplňte pole hello v okně tooServer hello připojit.</span><span class="sxs-lookup"><span data-stu-id="5c6ee-127">Fill in hello fields in hello Connect tooServer window.</span></span>
   
    ![Připojit tooServer][3]
   
   * <span data-ttu-id="5c6ee-129">**Název serveru**:</span><span class="sxs-lookup"><span data-stu-id="5c6ee-129">**Server name**.</span></span> <span data-ttu-id="5c6ee-130">Zadejte hello **název serveru** dříve identifikovat.</span><span class="sxs-lookup"><span data-stu-id="5c6ee-130">Enter hello **server name** previously identified.</span></span>
   * <span data-ttu-id="5c6ee-131">**Ověřování**:</span><span class="sxs-lookup"><span data-stu-id="5c6ee-131">**Authentication**.</span></span> <span data-ttu-id="5c6ee-132">Vyberte **Ověřování serveru SQL Server** nebo **Integrované ověřování Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="5c6ee-132">Select **SQL Server Authentication** or **Active Directory Integrated Authentication**.</span></span>
   * <span data-ttu-id="5c6ee-133">**Uživatelské jméno** a **Heslo**:</span><span class="sxs-lookup"><span data-stu-id="5c6ee-133">**User Name** and **Password**.</span></span> <span data-ttu-id="5c6ee-134">Pokud jste výše vybrali možnost Ověřování serveru SQL Server, zadejte uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="5c6ee-134">Enter user name and password if SQL Server Authentication was selected above.</span></span>
   * <span data-ttu-id="5c6ee-135">Klikněte na **Připojit**.</span><span class="sxs-lookup"><span data-stu-id="5c6ee-135">Click **Connect**.</span></span>
5. <span data-ttu-id="5c6ee-136">tooexplore, rozbalte položku serveru Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="5c6ee-136">tooexplore, expand your Azure SQL server.</span></span> <span data-ttu-id="5c6ee-137">Můžete zobrazit hello databáze přidružené k serveru hello.</span><span class="sxs-lookup"><span data-stu-id="5c6ee-137">You can view hello databases associated with hello server.</span></span> <span data-ttu-id="5c6ee-138">Rozbalte položku AdventureWorksDW toosee hello tabulky v ukázkové databázi.</span><span class="sxs-lookup"><span data-stu-id="5c6ee-138">Expand AdventureWorksDW toosee hello tables in your sample database.</span></span>
   
    ![Prozkoumejte AdventureWorksDW.][4]

## <a name="2-run-a-sample-query"></a><span data-ttu-id="5c6ee-140">2. Spuštění ukázkového dotazu</span><span class="sxs-lookup"><span data-stu-id="5c6ee-140">2. Run a sample query</span></span>
<span data-ttu-id="5c6ee-141">Teď, když připojení bylo navázáno tooyour databáze, můžete napsat dotaz.</span><span class="sxs-lookup"><span data-stu-id="5c6ee-141">Now that a connection has been established tooyour database, let's write a query.</span></span>

1. <span data-ttu-id="5c6ee-142">Pravým tlačítkem myši klikněte na databázi v Průzkumníku objektů systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="5c6ee-142">Right-click your database in SQL Server Object Explorer.</span></span>
2. <span data-ttu-id="5c6ee-143">Vyberte **Nový dotaz**.</span><span class="sxs-lookup"><span data-stu-id="5c6ee-143">Select **New Query**.</span></span> <span data-ttu-id="5c6ee-144">Otevře se nové okno dotazu.</span><span class="sxs-lookup"><span data-stu-id="5c6ee-144">A new query window opens.</span></span>
   
    ![Nový dotaz][5]
3. <span data-ttu-id="5c6ee-146">Zkopírujte tento dotaz TSQL do okna dotazu hello:</span><span class="sxs-lookup"><span data-stu-id="5c6ee-146">Copy this TSQL query into hello query window:</span></span>
   
    ```sql
    SELECT COUNT(*) FROM dbo.FactInternetSales;
    ```
4. <span data-ttu-id="5c6ee-147">Spusťte dotaz hello.</span><span class="sxs-lookup"><span data-stu-id="5c6ee-147">Run hello query.</span></span> <span data-ttu-id="5c6ee-148">toodo, klikněte na tlačítko hello zelenou šipku nebo použijte následující zástupce hello: `CTRL` + `SHIFT` + `E`.</span><span class="sxs-lookup"><span data-stu-id="5c6ee-148">toodo this, click hello green arrow or use hello following shortcut: `CTRL`+`SHIFT`+`E`.</span></span>
   
    ![Spuštění dotazu][6]
5. <span data-ttu-id="5c6ee-150">Podívejte se na výsledky dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="5c6ee-150">Look at hello query results.</span></span> <span data-ttu-id="5c6ee-151">V tomto příkladu má hello tabulka FactInternetSales 60 398 řádků.</span><span class="sxs-lookup"><span data-stu-id="5c6ee-151">In this example, hello FactInternetSales table has 60398 rows.</span></span>
   
    ![Výsledky dotazu][7]

## <a name="next-steps"></a><span data-ttu-id="5c6ee-153">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5c6ee-153">Next steps</span></span>
<span data-ttu-id="5c6ee-154">Teď, když se můžete připojit a dotazu, zkuste [vizualizace hello dat pomocí PowerBI][visualizing hello data with PowerBI].</span><span class="sxs-lookup"><span data-stu-id="5c6ee-154">Now that you can connect and query, try [visualizing hello data with PowerBI][visualizing hello data with PowerBI].</span></span>

<span data-ttu-id="5c6ee-155">tooconfigure prostředí pro ověřování Azure Active Directory, najdete v části [ověření tooSQL datového skladu][Authenticate tooSQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="5c6ee-155">tooconfigure your environment for Azure Active Directory authentication, see [Authenticate tooSQL Data Warehouse][Authenticate tooSQL Data Warehouse].</span></span>

<!--Arcticles-->
[Connect tooSQL Data Warehouse]: sql-data-warehouse-connect-overview.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Installing Visual Studio and SSDT]: sql-data-warehouse-install-visual-studio.md
[Authenticate tooSQL Data Warehouse]: sql-data-warehouse-authentication.md
[visualizing hello data with PowerBI]: sql-data-warehouse-get-started-visualize-with-power-bi.md  

<!--Other-->
[Azure portal]: https://portal.azure.com

<!--Image references-->

[1]: media/sql-data-warehouse-query-visual-studio/open-ssdt.png
[2]: media/sql-data-warehouse-query-visual-studio/add-server.png
[3]: media/sql-data-warehouse-query-visual-studio/connection-dialog.png
[4]: media/sql-data-warehouse-query-visual-studio/explore-sample.png
[5]: media/sql-data-warehouse-query-visual-studio/new-query2.png
[6]: media/sql-data-warehouse-query-visual-studio/run-query.png
[7]: media/sql-data-warehouse-query-visual-studio/query-results.png
