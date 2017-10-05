---
title: "Připojení Excelu k SQL Database | Dokumentace Microsoftu"
description: "Zjistěte, jak připojit Microsoft Excel k databázi SQL Azure v cloudu. Naimportujte si data do Excelu, kde můžete data dále zkoumat a vytvářet z nich sestavy."
services: sql-database
keywords: "připojení excelu k sql, import dat do excelu"
documentationcenter: 
author: joseidz
manager: jhubbard
editor: 
ms.assetid: 906924bc-2707-48d3-bac6-397976a0409d
ms.service: sql-database
ms.custom: develop apps
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/10/2017
ms.author: jhubbard
ms.openlocfilehash: 97344d7c0be38b3092a3224074d486b5bb984176
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="connect-excel-to-an-azure-sql-database-and-create-a-report"></a><span data-ttu-id="3094c-105">Připojení Excelu k databázi Azure SQL a vytvoření sestavy</span><span class="sxs-lookup"><span data-stu-id="3094c-105">Connect Excel to an Azure SQL database and create a report</span></span>

<span data-ttu-id="3094c-106">Připojení Excelu k databázi SQL v cloudu a importovat data a vytvářet tabulky a grafy na základě hodnot v databázi.</span><span class="sxs-lookup"><span data-stu-id="3094c-106">Connect Excel to a SQL database in the cloud and import data and create tables and charts based on values in the database.</span></span> <span data-ttu-id="3094c-107">V tomto kurzu nastavíte připojení mezi Excelem a databázovou tabulkou, uložíte soubor, ve kterém jsou uložená data a informace o připojení pro Excel, a pak z hodnot v databázi vytvoříte kontingenční graf.</span><span class="sxs-lookup"><span data-stu-id="3094c-107">In this tutorial you will set up the connection between Excel and a database table, save the file that stores data and the connection information for Excel, and then create a pivot chart from the database values.</span></span>

<span data-ttu-id="3094c-108">Než začnete, budete potřebovat databázi SQL v Azure.</span><span class="sxs-lookup"><span data-stu-id="3094c-108">You'll need a SQL database in Azure before you get started.</span></span> <span data-ttu-id="3094c-109">Pokud žádnou nemáte, podívejte se na téma [Vytvoření první databáze SQL](sql-database-get-started-portal.md), kde najdete informace o tom, jak si během několika málo minut zprovoznit databázi s ukázkovými daty.</span><span class="sxs-lookup"><span data-stu-id="3094c-109">If you don't have one, see [Create your first SQL database](sql-database-get-started-portal.md) to get a database with sample data up and running in a few minutes.</span></span> <span data-ttu-id="3094c-110">V tomto článku budete importovat ukázková data do aplikace Excel z tohoto článku, ale můžete provést kroky pro podobné s vlastními daty.</span><span class="sxs-lookup"><span data-stu-id="3094c-110">In this article, you'll import sample data into Excel from that article, but you can follow similar steps with your own data.</span></span>

<span data-ttu-id="3094c-111">Budete také potřebovat Excel.</span><span class="sxs-lookup"><span data-stu-id="3094c-111">You'll also need a copy of Excel.</span></span> <span data-ttu-id="3094c-112">V tomto článku používáme [Microsoft Excel 2016](https://products.office.com/).</span><span class="sxs-lookup"><span data-stu-id="3094c-112">This article uses [Microsoft Excel 2016](https://products.office.com/).</span></span>

## <a name="connect-excel-to-a-sql-database-and-create-an-odc-file"></a><span data-ttu-id="3094c-113">Připojení Excelu k databázi SQL a vytvoření souboru odc</span><span class="sxs-lookup"><span data-stu-id="3094c-113">Connect Excel to a SQL database and create an odc file</span></span>
1. <span data-ttu-id="3094c-114">Když budete chtít Excel připojit k databázi SQL, otevřete ho a vytvořte nový sešit nebo otevřete existující excelový sešit.</span><span class="sxs-lookup"><span data-stu-id="3094c-114">To connect Excel to SQL database, open Excel and then create a new workbook or open an existing Excel workbook.</span></span>
2. <span data-ttu-id="3094c-115">V řádku nabídek v horní části stránky klikněte na **Data**, **Z jiných zdrojů** a potom klikněte na **Z SQL Serveru**.</span><span class="sxs-lookup"><span data-stu-id="3094c-115">In the menu bar at the top of the page click **Data**, click **From Other Sources**, and then click **From SQL Server**.</span></span>
   
   ![Výběr zdroje dat: Připojení Excelu k databázi SQL](./media/sql-database-connect-excel/excel_data_source.png)
   
   <span data-ttu-id="3094c-117">Otevře se Průvodce datovým připojením.</span><span class="sxs-lookup"><span data-stu-id="3094c-117">The Data Connection Wizard opens.</span></span>
3. <span data-ttu-id="3094c-118">V dialogovém okně **Připojit k databázovému serveru** zadejte **název serveru** SQL Database, ke kterému se chcete připojit, v následující podobě: <*název_serveru*>**. database.windows.net**.</span><span class="sxs-lookup"><span data-stu-id="3094c-118">In the **Connect to Database Server** dialog box, type the SQL Database **Server name** you want to connect to in the form <*servername*>**.database.windows.net**.</span></span> <span data-ttu-id="3094c-119">Například **adworkserver.database.windows.net**.</span><span class="sxs-lookup"><span data-stu-id="3094c-119">For example, **adworkserver.database.windows.net**.</span></span>
4. <span data-ttu-id="3094c-120">V části **Přihlašovací pověření** klikněte na **Použít následující uživatelské jméno a heslo**, zadejte **uživatelské jméno** a **heslo**, které jste nastavili pro server SQL Database, když jste ho vytvářeli, a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="3094c-120">Under **Log on credentials**, click **Use the following User Name and Password**, type the **User Name** and **Password** you set up for the SQL Database server when you created it, and then click **Next**.</span></span>
   
   ![Zadejte název a přihlašovací údaje serveru.](./media/sql-database-connect-excel/connect-to-server.png)
   
   > [!TIP]
   > <span data-ttu-id="3094c-122">V závislosti na vašem síťovém prostředí nemusíte být schopni se připojit nebo může dojít ke ztrátě připojení (pokud server databáze SQL nepovoluje přenos dat z IP adresy vašeho klienta).</span><span class="sxs-lookup"><span data-stu-id="3094c-122">Depending on your network environment, you may not be able to connect or you may lose the connection if the SQL Database server doesn't allow traffic from your client IP address.</span></span> <span data-ttu-id="3094c-123">Přejděte na [portál Azure](https://portal.azure.com/), klikněte na SQL servery, klikněte na svůj server, v nastavení klikněte na bránu firewall a přidejte IP adresu svého klienta.</span><span class="sxs-lookup"><span data-stu-id="3094c-123">Go to the [Azure portal](https://portal.azure.com/), click SQL servers, click your server, click firewall under settings and add your client IP address.</span></span> <span data-ttu-id="3094c-124">Podrobnosti najdete v tématu [Jak nakonfigurovat nastavení brány firewall](sql-database-configure-firewall-settings.md).</span><span class="sxs-lookup"><span data-stu-id="3094c-124">See [How to configure firewall settings](sql-database-configure-firewall-settings.md) for details.</span></span>
   > 
   > 
5. <span data-ttu-id="3094c-125">V dialogovém okně **Vybrat databázi a tabulku** vyberte ze seznamu databázi, se kterou chcete pracovat, a potom klikněte na tabulky nebo zobrazení, se kterými chcete pracovat (my jsme zvolili **vGetAllCategories**). Potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="3094c-125">In the **Select Database and Table** dialog, select the database you want to work with from the list, and then click the tables or views you want to work with (we chose **vGetAllCategories**), and then click **Next**.</span></span>
   
    ![Vyberte databázi a tabulku.](./media/sql-database-connect-excel/select-database-and-table.png)
   
    <span data-ttu-id="3094c-127">Otevře se dialogové okno **Uložit soubor datového připojení a dokončit**, kde zadáte informace o souboru databázového připojení (*.odc) Office, který používá Excel.</span><span class="sxs-lookup"><span data-stu-id="3094c-127">The **Save Data Connection File and Finish** dialog box opens, where you provide information about the Office database connection (*.odc) file that Excel uses.</span></span> <span data-ttu-id="3094c-128">Můžete nechat nastavené výchozí hodnoty nebo si vybrané možnosti přizpůsobit.</span><span class="sxs-lookup"><span data-stu-id="3094c-128">You can leave the defaults or customize your selections.</span></span>
6. <span data-ttu-id="3094c-129">Můžete nechat nastavené výchozí hodnoty, ale důležité je poznamenat si **název souboru**.</span><span class="sxs-lookup"><span data-stu-id="3094c-129">You can leave the defaults, but note the **File Name** in particular.</span></span> <span data-ttu-id="3094c-130">**Popis**, **popisný název** a **klíčová slova pro vyhledávání** vám a dalším uživatelům pomůžou zapamatovat si, k jaké databázi se připojujete, a najít připojení.</span><span class="sxs-lookup"><span data-stu-id="3094c-130">A **Description**, a **Friendly Name**, and **Search Keywords** help you and other users remember what you're connecting to and find the connection.</span></span> <span data-ttu-id="3094c-131">Pokud chcete, aby se informace o připojení uložily do souboru odc, aby se mohl aktualizovat, když se k němu připojíte, klikněte na **Vždy aktualizovat data pomocí tohoto souboru** a potom klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="3094c-131">Click **Always attempt to use this file to refresh data** if you want connection information stored in the odc file so it can update when you connect to it, and then click **Finish**.</span></span>
   
    ![Uložení souboru odc](./media/sql-database-connect-excel/save-odc-file.png)
   
    <span data-ttu-id="3094c-133">Zobrazí se dialogové okno **Importovat data**.</span><span class="sxs-lookup"><span data-stu-id="3094c-133">The **Import data** dialog box appears.</span></span>

## <a name="import-the-data-into-excel-and-create-a-pivot-chart"></a><span data-ttu-id="3094c-134">Import dat do Excelu a vytvoření kontingenčního grafu</span><span class="sxs-lookup"><span data-stu-id="3094c-134">Import the data into Excel and create a pivot chart</span></span>
<span data-ttu-id="3094c-135">Máte vytvořené připojení a také soubor s daty a informacemi o připojení, můžete tedy začít s importem dat.</span><span class="sxs-lookup"><span data-stu-id="3094c-135">Now that you've established the connection and created the file with data and connection information, you're reading to import the data.</span></span>

1. <span data-ttu-id="3094c-136">V dialogovém okně **Importovat data** klikněte na možnost, kterou chcete použít pro prezentování vašich dat v listu, a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="3094c-136">In the **Import Data** dialog, click the option you want for presenting your data in the worksheet and then click **OK**.</span></span> <span data-ttu-id="3094c-137">Zvolili jsme možnost **Kontingenční graf**.</span><span class="sxs-lookup"><span data-stu-id="3094c-137">We chose **PivotChart**.</span></span> <span data-ttu-id="3094c-138">Můžete také zvolit možnost **Nový list** nebo **Přidat tato data do datového modelu**.</span><span class="sxs-lookup"><span data-stu-id="3094c-138">You can also choose to create a **New worksheet** or to **Add this data to a Data Model**.</span></span> <span data-ttu-id="3094c-139">Další informace o datových modelech najdete v tématu [Vytvoření datového modelu v Excelu](https://support.office.com/article/Create-a-Data-Model-in-Excel-87E7A54C-87DC-488E-9410-5C75DBCB0F7B).</span><span class="sxs-lookup"><span data-stu-id="3094c-139">For more information on Data Models, see [Create a data model in Excel](https://support.office.com/article/Create-a-Data-Model-in-Excel-87E7A54C-87DC-488E-9410-5C75DBCB0F7B).</span></span> <span data-ttu-id="3094c-140">Klikněte na **Vlastnosti** a prozkoumejte informace o souboru odc, který jste vytvořili v předchozím kroku. Pak zvolte možnosti pro aktualizaci dat.</span><span class="sxs-lookup"><span data-stu-id="3094c-140">Click **Properties** to explore information about the odc file you created in the previous step and to choose options for refreshing the data.</span></span>
   
    ![Volba formátu pro data v Excelu](./media/sql-database-connect-excel/import-data.png)
   
    <span data-ttu-id="3094c-142">List má teď prázdnou kontingenční tabulku a graf.</span><span class="sxs-lookup"><span data-stu-id="3094c-142">The worksheet now has an empty pivot table and chart.</span></span>
2. <span data-ttu-id="3094c-143">V části **Pole kontingenční tabulky** zaškrtněte políčka pro všechna pole, která chcete zobrazit.</span><span class="sxs-lookup"><span data-stu-id="3094c-143">Under **PivotTable Fields**, select all the check-boxes for the fields you want to view.</span></span>
   
    ![Nakonfigurujte sestavu databáze.](./media/sql-database-connect-excel/power-pivot-results.png)

> [!TIP]
> <span data-ttu-id="3094c-145">Pokud chcete k databázi připojit jiné excelové sešity a listy, klikněte na **Data**, klikněte na **Připojení**, klikněte na **Přidat**, ze seznamu zvolte připojení, které jste vytvořili, a pak klikněte na **Otevřít**.</span><span class="sxs-lookup"><span data-stu-id="3094c-145">If you want to connect other Excel workbooks and worksheets to the database, click **Data**, click **Connections**, click **Add**, choose the connection you created from the list, and then click **Open**.</span></span>
> <span data-ttu-id="3094c-146">![Otevření připojení z jiného sešitu](./media/sql-database-connect-excel/open-from-another-workbook.png)</span><span class="sxs-lookup"><span data-stu-id="3094c-146">![Open a connection from another workbook](./media/sql-database-connect-excel/open-from-another-workbook.png)</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="3094c-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3094c-147">Next steps</span></span>
* <span data-ttu-id="3094c-148">Zjistěte, jak se [připojit k SQL Database přes SQL Server Management Studio](sql-database-connect-query-ssms.md) a provádět pokročilé dotazy a analýzy.</span><span class="sxs-lookup"><span data-stu-id="3094c-148">Learn how to [Connect to SQL Database with SQL Server Management Studio](sql-database-connect-query-ssms.md) for advanced querying and analysis.</span></span>
* <span data-ttu-id="3094c-149">Další informace o výhodách [elastických fondů](sql-database-elastic-pool.md).</span><span class="sxs-lookup"><span data-stu-id="3094c-149">Learn about the benefits of [elastic pools](sql-database-elastic-pool.md).</span></span>
* <span data-ttu-id="3094c-150">Zjistěte, jak [vytvořit webovou aplikaci, která se připojuje k SQL Database na back-endu](../app-service-web/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="3094c-150">Learn how to [create a web application that connects to SQL Database on the back-end](../app-service-web/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md).</span></span>

