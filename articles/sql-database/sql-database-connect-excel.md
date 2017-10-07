---
title: "aaaConnect Excel tooSQL databáze | Microsoft Docs"
description: "Zjistěte, jak databázi aplikace Microsoft Excel tooAzure tooconnect SQL v cloudu hello. Naimportujte si data do Excelu, kde můžete data dále zkoumat a vytvářet z nich sestavy."
services: sql-database
keywords: "připojení aplikace excel toosql, naimportujte tooexcel dat"
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
ms.openlocfilehash: 0048849432023145bd1009d45b6d9b64a9c7ac3c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-excel-tooan-azure-sql-database-and-create-a-report"></a><span data-ttu-id="c6a17-105">Připojení aplikace Excel tooan Azure SQL database a vytvoření sestavy</span><span class="sxs-lookup"><span data-stu-id="c6a17-105">Connect Excel tooan Azure SQL database and create a report</span></span>

<span data-ttu-id="c6a17-106">Připojení aplikace Excel tooa databáze SQL v cloudu hello a importovat data a vytvářet tabulky a grafy na základě hodnot v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="c6a17-106">Connect Excel tooa SQL database in hello cloud and import data and create tables and charts based on values in hello database.</span></span> <span data-ttu-id="c6a17-107">V tomto kurzu nastavíte hello připojení mezi Excelem a databázovou tabulku uložte soubor hello, která ukládá data a hello informace o připojení pro Excel a pak vytvoříte kontingenční graf z hello hodnot v databázi.</span><span class="sxs-lookup"><span data-stu-id="c6a17-107">In this tutorial you will set up hello connection between Excel and a database table, save hello file that stores data and hello connection information for Excel, and then create a pivot chart from hello database values.</span></span>

<span data-ttu-id="c6a17-108">Než začnete, budete potřebovat databázi SQL v Azure.</span><span class="sxs-lookup"><span data-stu-id="c6a17-108">You'll need a SQL database in Azure before you get started.</span></span> <span data-ttu-id="c6a17-109">Pokud nemáte, přečtěte si téma [vytvořit svoji první databázi SQL](sql-database-get-started-portal.md) tooget databázi s ukázkovými daty fungovaly za pár minut.</span><span class="sxs-lookup"><span data-stu-id="c6a17-109">If you don't have one, see [Create your first SQL database](sql-database-get-started-portal.md) tooget a database with sample data up and running in a few minutes.</span></span> <span data-ttu-id="c6a17-110">V tomto článku budete importovat ukázková data do aplikace Excel z tohoto článku, ale můžete provést kroky pro podobné s vlastními daty.</span><span class="sxs-lookup"><span data-stu-id="c6a17-110">In this article, you'll import sample data into Excel from that article, but you can follow similar steps with your own data.</span></span>

<span data-ttu-id="c6a17-111">Budete také potřebovat Excel.</span><span class="sxs-lookup"><span data-stu-id="c6a17-111">You'll also need a copy of Excel.</span></span> <span data-ttu-id="c6a17-112">V tomto článku používáme [Microsoft Excel 2016](https://products.office.com/).</span><span class="sxs-lookup"><span data-stu-id="c6a17-112">This article uses [Microsoft Excel 2016](https://products.office.com/).</span></span>

## <a name="connect-excel-tooa-sql-database-and-create-an-odc-file"></a><span data-ttu-id="c6a17-113">Připojení aplikace Excel tooa SQL database a vytvoření souboru odc</span><span class="sxs-lookup"><span data-stu-id="c6a17-113">Connect Excel tooa SQL database and create an odc file</span></span>
1. <span data-ttu-id="c6a17-114">tooconnect Excel tooSQL databáze, otevřete aplikaci Excel a vytvoření nového sešitu nebo otevřít existující sešit aplikace Excel.</span><span class="sxs-lookup"><span data-stu-id="c6a17-114">tooconnect Excel tooSQL database, open Excel and then create a new workbook or open an existing Excel workbook.</span></span>
2. <span data-ttu-id="c6a17-115">V řádku nabídek hello hello horní části stránky hello klikněte na **Data**, klikněte na tlačítko **z jiných zdrojů**a potom klikněte na **z SQL serveru**.</span><span class="sxs-lookup"><span data-stu-id="c6a17-115">In hello menu bar at hello top of hello page click **Data**, click **From Other Sources**, and then click **From SQL Server**.</span></span>
   
   ![Vyberte zdroj dat: připojení Excelu tooSQL databáze.](./media/sql-database-connect-excel/excel_data_source.png)
   
   <span data-ttu-id="c6a17-117">Otevře se Průvodce datovým připojením Hello.</span><span class="sxs-lookup"><span data-stu-id="c6a17-117">hello Data Connection Wizard opens.</span></span>
3. <span data-ttu-id="c6a17-118">V hello **připojit tooDatabase Server** dialogové okno, hello typ SQL Database **název serveru** chcete tooconnect tooin hello formulář <*servername* > **. database.windows.net**.</span><span class="sxs-lookup"><span data-stu-id="c6a17-118">In hello **Connect tooDatabase Server** dialog box, type hello SQL Database **Server name** you want tooconnect tooin hello form <*servername*>**.database.windows.net**.</span></span> <span data-ttu-id="c6a17-119">Například **adworkserver.database.windows.net**.</span><span class="sxs-lookup"><span data-stu-id="c6a17-119">For example, **adworkserver.database.windows.net**.</span></span>
4. <span data-ttu-id="c6a17-120">V části **přihlašovací pověření**, klikněte na tlačítko **hello použijte následující uživatelské jméno a heslo**, typ hello **uživatelské jméno** a **heslo** nastavil pro Databáze SQL serveru text Hello, pokud jste ji vytvořili a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="c6a17-120">Under **Log on credentials**, click **Use hello following User Name and Password**, type hello **User Name** and **Password** you set up for hello SQL Database server when you created it, and then click **Next**.</span></span>
   
   ![Zadejte název a přihlašovací údaje serveru hello](./media/sql-database-connect-excel/connect-to-server.png)
   
   > [!TIP]
   > <span data-ttu-id="c6a17-122">V závislosti na vašem síťovém prostředí nemusí být možné tooconnect nebo můžete ztratit připojení hello, pokud server hello databáze SQL nepovoluje přenos dat z IP adresy vašeho klienta.</span><span class="sxs-lookup"><span data-stu-id="c6a17-122">Depending on your network environment, you may not be able tooconnect or you may lose hello connection if hello SQL Database server doesn't allow traffic from your client IP address.</span></span> <span data-ttu-id="c6a17-123">Přejděte toohello [portál Azure](https://portal.azure.com/), klikněte na SQL servery, klikněte na svůj server, klikněte v části nastavení brány firewall a přidejte IP adresu svého klienta.</span><span class="sxs-lookup"><span data-stu-id="c6a17-123">Go toohello [Azure portal](https://portal.azure.com/), click SQL servers, click your server, click firewall under settings and add your client IP address.</span></span> <span data-ttu-id="c6a17-124">V tématu [jak nastavení brány firewall tooconfigure](sql-database-configure-firewall-settings.md) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="c6a17-124">See [How tooconfigure firewall settings](sql-database-configure-firewall-settings.md) for details.</span></span>
   > 
   > 
5. <span data-ttu-id="c6a17-125">V hello **vybrat databázi a tabulku** dialogové okno, vyberte hello databázi toowork s hello seznamu a pak klikněte na tlačítko hello tabulek nebo zobrazení, které chcete toowork s (jsme zvolili **vGetAllCategories**) a potom Klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="c6a17-125">In hello **Select Database and Table** dialog, select hello database you want toowork with from hello list, and then click hello tables or views you want toowork with (we chose **vGetAllCategories**), and then click **Next**.</span></span>
   
    ![Vyberte databázi a tabulku.](./media/sql-database-connect-excel/select-database-and-table.png)
   
    <span data-ttu-id="c6a17-127">Hello **uložit soubor datového připojení a dokončit** otevře se dialogové okno, kde zadáte informace o hello Office databáze připojení (*.odc) souboru, který používá Excel.</span><span class="sxs-lookup"><span data-stu-id="c6a17-127">hello **Save Data Connection File and Finish** dialog box opens, where you provide information about hello Office database connection (*.odc) file that Excel uses.</span></span> <span data-ttu-id="c6a17-128">Můžete ponechat výchozí nastavení hello nebo si vybrané možnosti přizpůsobit.</span><span class="sxs-lookup"><span data-stu-id="c6a17-128">You can leave hello defaults or customize your selections.</span></span>
6. <span data-ttu-id="c6a17-129">Můžete ponechat výchozí hodnoty hello, ale Poznámka hello **název souboru** na konkrétní.</span><span class="sxs-lookup"><span data-stu-id="c6a17-129">You can leave hello defaults, but note hello **File Name** in particular.</span></span> <span data-ttu-id="c6a17-130">A **popis**, **popisný název**, a **hledaná klíčová slova** vám pomůže a ostatní uživatelé mějte na paměti, co se připojujete tooand najít hello připojení.</span><span class="sxs-lookup"><span data-stu-id="c6a17-130">A **Description**, a **Friendly Name**, and **Search Keywords** help you and other users remember what you're connecting tooand find hello connection.</span></span> <span data-ttu-id="c6a17-131">Klikněte na tlačítko **vždy pokusí toouse tato data souboru toorefresh** Pokud chcete informace o připojení, které jsou uložené v souboru odc hello, aby se mohl aktualizovat při připojení tooit a pak klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="c6a17-131">Click **Always attempt toouse this file toorefresh data** if you want connection information stored in hello odc file so it can update when you connect tooit, and then click **Finish**.</span></span>
   
    ![Uložení souboru odc](./media/sql-database-connect-excel/save-odc-file.png)
   
    <span data-ttu-id="c6a17-133">Hello **importovat data** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c6a17-133">hello **Import data** dialog box appears.</span></span>

## <a name="import-hello-data-into-excel-and-create-a-pivot-chart"></a><span data-ttu-id="c6a17-134">Umožňuje importovat hello data do Excelu a vytvoření kontingenčního grafu</span><span class="sxs-lookup"><span data-stu-id="c6a17-134">Import hello data into Excel and create a pivot chart</span></span>
<span data-ttu-id="c6a17-135">Teď, když jste vytvořili připojení hello a soubor vytvořený hello se data a informace o připojení, při čtení dat tooimport hello.</span><span class="sxs-lookup"><span data-stu-id="c6a17-135">Now that you've established hello connection and created hello file with data and connection information, you're reading tooimport hello data.</span></span>

1. <span data-ttu-id="c6a17-136">V hello **importovat Data** dialogové okno, klikněte na možnost hello chcete použít pro prezentování vašich dat v listu hello a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="c6a17-136">In hello **Import Data** dialog, click hello option you want for presenting your data in hello worksheet and then click **OK**.</span></span> <span data-ttu-id="c6a17-137">Zvolili jsme možnost **Kontingenční graf**.</span><span class="sxs-lookup"><span data-stu-id="c6a17-137">We chose **PivotChart**.</span></span> <span data-ttu-id="c6a17-138">Můžete také toocreate **nový list** nebo příliš**přidat tato data tooa datový Model**.</span><span class="sxs-lookup"><span data-stu-id="c6a17-138">You can also choose toocreate a **New worksheet** or too**Add this data tooa Data Model**.</span></span> <span data-ttu-id="c6a17-139">Další informace o datových modelech najdete v tématu [Vytvoření datového modelu v Excelu](https://support.office.com/article/Create-a-Data-Model-in-Excel-87E7A54C-87DC-488E-9410-5C75DBCB0F7B).</span><span class="sxs-lookup"><span data-stu-id="c6a17-139">For more information on Data Models, see [Create a data model in Excel](https://support.office.com/article/Create-a-Data-Model-in-Excel-87E7A54C-87DC-488E-9410-5C75DBCB0F7B).</span></span> <span data-ttu-id="c6a17-140">Klikněte na tlačítko **vlastnosti** tooexplore informace o souboru odc hello jste vytvořili v hello předchozí krok a toochoose možnosti pro aktualizaci dat hello.</span><span class="sxs-lookup"><span data-stu-id="c6a17-140">Click **Properties** tooexplore information about hello odc file you created in hello previous step and toochoose options for refreshing hello data.</span></span>
   
    ![Výběr hello formát dat v aplikaci Excel](./media/sql-database-connect-excel/import-data.png)
   
    <span data-ttu-id="c6a17-142">Hello list má teď prázdnou kontingenční tabulku a graf.</span><span class="sxs-lookup"><span data-stu-id="c6a17-142">hello worksheet now has an empty pivot table and chart.</span></span>
2. <span data-ttu-id="c6a17-143">V části **pole kontingenční tabulky**, vyberte všechny hello zaškrtněte políčka pro hello chcete tooview pole.</span><span class="sxs-lookup"><span data-stu-id="c6a17-143">Under **PivotTable Fields**, select all hello check-boxes for hello fields you want tooview.</span></span>
   
    ![Nakonfigurujte sestavu databáze.](./media/sql-database-connect-excel/power-pivot-results.png)

> [!TIP]
> <span data-ttu-id="c6a17-145">Pokud chcete tooconnect jiné databáze pro toohello sešity a listy aplikace Excel, klikněte na tlačítko **Data**, klikněte na tlačítko **připojení**, klikněte na tlačítko **přidat**, zvolte hello připojení, které jste vytvořili ze seznamu hello a pak klikněte na tlačítko **otevřete**.</span><span class="sxs-lookup"><span data-stu-id="c6a17-145">If you want tooconnect other Excel workbooks and worksheets toohello database, click **Data**, click **Connections**, click **Add**, choose hello connection you created from hello list, and then click **Open**.</span></span>
> <span data-ttu-id="c6a17-146">![Otevření připojení z jiného sešitu](./media/sql-database-connect-excel/open-from-another-workbook.png)</span><span class="sxs-lookup"><span data-stu-id="c6a17-146">![Open a connection from another workbook](./media/sql-database-connect-excel/open-from-another-workbook.png)</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="c6a17-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c6a17-147">Next steps</span></span>
* <span data-ttu-id="c6a17-148">Zjistěte, jak příliš[připojit tooSQL databáze s SQL Server Management Studio](sql-database-connect-query-ssms.md) pro pokročilé dotazy a analýzy.</span><span class="sxs-lookup"><span data-stu-id="c6a17-148">Learn how too[Connect tooSQL Database with SQL Server Management Studio](sql-database-connect-query-ssms.md) for advanced querying and analysis.</span></span>
* <span data-ttu-id="c6a17-149">Další informace o výhodách hello [elastické fondy](sql-database-elastic-pool.md).</span><span class="sxs-lookup"><span data-stu-id="c6a17-149">Learn about hello benefits of [elastic pools](sql-database-elastic-pool.md).</span></span>
* <span data-ttu-id="c6a17-150">Zjistěte, jak příliš[vytvořit webovou aplikaci, která se připojuje tooSQL databáze na hello back-end](../app-service-web/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="c6a17-150">Learn how too[create a web application that connects tooSQL Database on hello back-end](../app-service-web/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md).</span></span>

