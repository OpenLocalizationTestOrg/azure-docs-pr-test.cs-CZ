---
title: "aaaUse Redgate tooload data tooyour Azure datového skladu | Microsoft Docs"
description: "Zjistěte, jak Studio platformy toouse Redgate dat pro scénáře datových skladů."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: 
ms.assetid: 670aef98-31f7-4436-86c0-cc989a39fe7f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 10/31/2016
ms.author: cakarst;barbkess
ms.openlocfilehash: 6082390c07c8ffa73ebd8ab272ace00ba8bb1897
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-with-redgate-data-platform-studio"></a><span data-ttu-id="7dcd6-103">Načtení dat pomocí nástroje Redgate Data Platform Studio</span><span class="sxs-lookup"><span data-stu-id="7dcd6-103">Load data with Redgate Data Platform Studio</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="7dcd6-104">Redgate</span><span class="sxs-lookup"><span data-stu-id="7dcd6-104">Redgate</span></span>](sql-data-warehouse-load-with-redgate.md)
> * [<span data-ttu-id="7dcd6-105">Data Factory</span><span class="sxs-lookup"><span data-stu-id="7dcd6-105">Data Factory</span></span>](sql-data-warehouse-get-started-load-with-azure-data-factory.md)
> * [<span data-ttu-id="7dcd6-106">PolyBase</span><span class="sxs-lookup"><span data-stu-id="7dcd6-106">PolyBase</span></span>](sql-data-warehouse-get-started-load-with-polybase.md)
> * [<span data-ttu-id="7dcd6-107">BCP</span><span class="sxs-lookup"><span data-stu-id="7dcd6-107">BCP</span></span>](sql-data-warehouse-load-with-bcp.md)
> 
> 

<span data-ttu-id="7dcd6-108">Tento kurz ukazuje, jak toouse [Redgate na Data platformy Studio](http://www.red-gate.com/products/azure-development/data-platform-studio/) (distribučních bodů) toomove data ze tooAzure místní systém SQL Server SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="7dcd6-108">This tutorial shows you how toouse [Redgate's Data Platform Studio](http://www.red-gate.com/products/azure-development/data-platform-studio/) (DPS) toomove data from an on-premises SQL Server tooAzure SQL Data Warehouse.</span></span> <span data-ttu-id="7dcd6-109">Data platformy Studio platí hello nejvhodnější kompatibility opravy a optimalizace, takže má hello nejrychleji tooget způsob, jak začít s SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="7dcd6-109">Data Platform Studio applies hello most appropriate compatibility fixes and optimizations, so it's hello quickest way tooget started with SQL Data Warehouse.</span></span>

> [!NOTE]
> <span data-ttu-id="7dcd6-110">[Redgate](http://www.red-gate.com) je dlouhodobý partner Microsoftu, který poskytuje různé nástroje pro SQL Server.</span><span class="sxs-lookup"><span data-stu-id="7dcd6-110">[Redgate](http://www.red-gate.com) is a long-time Microsoft partner that delivers various SQL Server tools.</span></span> <span data-ttu-id="7dcd6-111">Tato funkce byla v nástroji Data Platform Studio zpřístupněna bezplatně pro komerční i nekomerční použití.</span><span class="sxs-lookup"><span data-stu-id="7dcd6-111">This feature in Data Platform Studio has been made available freely for both commercial and non-commercial use.</span></span>
> 
> 

## <a name="before-you-begin"></a><span data-ttu-id="7dcd6-112">Než začnete</span><span class="sxs-lookup"><span data-stu-id="7dcd6-112">Before you begin</span></span>
### <a name="create-or-identify-resources"></a><span data-ttu-id="7dcd6-113">Vytvoření nebo určení prostředků</span><span class="sxs-lookup"><span data-stu-id="7dcd6-113">Create or identify resources</span></span>
<span data-ttu-id="7dcd6-114">Před zahájením tohoto kurzu potřebujete toohave:</span><span class="sxs-lookup"><span data-stu-id="7dcd6-114">Before starting this tutorial, you need toohave:</span></span>

* <span data-ttu-id="7dcd6-115">**místní databáze SQL serveru**: hello data, která chcete tooimport tooSQL datového skladu musí toocome z místního serveru SQL (verze 2008 R2 nebo novější).</span><span class="sxs-lookup"><span data-stu-id="7dcd6-115">**on-premises SQL Server Database**: hello data you want tooimport tooSQL Data Warehouse needs toocome from an on-premises SQL Server (version 2008R2 or above).</span></span> <span data-ttu-id="7dcd6-116">Data Platform Studio nemůže importovat data přímo z Azure SQL Database nebo z textových souborů.</span><span class="sxs-lookup"><span data-stu-id="7dcd6-116">Data Platform Studio cannot import data directly from an Azure SQL Database or from text files.</span></span>
* <span data-ttu-id="7dcd6-117">**Účet služby Azure Storage**: Studio platformy Data zpracuje hello data v Azure Blob Storage před načtením do SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="7dcd6-117">**Azure Storage Account**: Data Platform Studio stages hello data in Azure Blob Storage before loading it into SQL Data Warehouse.</span></span> <span data-ttu-id="7dcd6-118">účet úložiště Hello musí používat model nasazení hello "Resource Manager" (výchozí hello) namísto hello "Classic" modelu nasazení.</span><span class="sxs-lookup"><span data-stu-id="7dcd6-118">hello storage account must be using hello “Resource Manager” deployment model (hello default) rather than hello “Classic” deployment model.</span></span> <span data-ttu-id="7dcd6-119">Pokud nemáte účet úložiště, zjistěte, jak tooCreate účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="7dcd6-119">If you don't have a storage account, learn how tooCreate a storage account.</span></span> 
* <span data-ttu-id="7dcd6-120">**SQL Data Warehouse**: v tomto kurzu přesune hello data z místního serveru SQL Server tooSQL datového skladu, proto musíte toohave online datový sklad.</span><span class="sxs-lookup"><span data-stu-id="7dcd6-120">**SQL Data Warehouse**: This tutorial moves hello data from on-premises SQL Server tooSQL Data Warehouse, so you need toohave a data warehouse online.</span></span> <span data-ttu-id="7dcd6-121">Pokud již datový sklad nemáte, zjistěte, jak tooCreate Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="7dcd6-121">If you do not already have a data warehouse, learn how tooCreate an Azure SQL Data Warehouse.</span></span>

> [!NOTE]
> <span data-ttu-id="7dcd6-122">Výkon je vyšší, pokud účet úložiště hello hello datového skladu jsou vytvořeny a hello stejné oblasti.</span><span class="sxs-lookup"><span data-stu-id="7dcd6-122">Performance is improved if hello storage account and hello data warehouse are created in hello same region.</span></span>
> 
> 

## <a name="step-1-sign-in-toodata-platform-studio-with-your-azure-account"></a><span data-ttu-id="7dcd6-123">Krok 1: Zaregistrujte v tooData Studio platformy s vaším účtem Azure</span><span class="sxs-lookup"><span data-stu-id="7dcd6-123">Step 1: Sign in tooData Platform Studio with your Azure account</span></span>
<span data-ttu-id="7dcd6-124">Otevřete webový prohlížeč a přejděte toohello [Data platformy Studio](https://www.dataplatformstudio.com/) webu.</span><span class="sxs-lookup"><span data-stu-id="7dcd6-124">Open your web browser and navigate toohello [Data Platform Studio](https://www.dataplatformstudio.com/) website.</span></span> <span data-ttu-id="7dcd6-125">Přihlášení s hello stejný účet Azure, že můžete použít toocreate hello úložiště účet a datový sklad.</span><span class="sxs-lookup"><span data-stu-id="7dcd6-125">Sign in with hello same Azure account that you used toocreate hello storage account and data warehouse.</span></span> <span data-ttu-id="7dcd6-126">Pokud vaše e-mailová adresa je přidružen pracovní nebo školní účet a účet Microsoft, být jisti toochoose hello účet, který má přístup k prostředkům tooyour.</span><span class="sxs-lookup"><span data-stu-id="7dcd6-126">If your email address is associated with both a work or school account and a Microsoft account, be sure toochoose hello account that has access tooyour resources.</span></span>

> [!NOTE]
> <span data-ttu-id="7dcd6-127">Pokud používáte Data platformy Studio poprvé, budete vyzváni toomanage oprávnění aplikace hello toogrant vašich prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="7dcd6-127">If this is your first time using Data Platform Studio, you are asked toogrant hello application permission toomanage your Azure resources.</span></span>
> 
> 

## <a name="step-2-start-hello-import-wizard"></a><span data-ttu-id="7dcd6-128">Krok 2: Spuštění Průvodce importem hello</span><span class="sxs-lookup"><span data-stu-id="7dcd6-128">Step 2: Start hello Import Wizard</span></span>
<span data-ttu-id="7dcd6-129">Z hlavní obrazovky hello distribučních bodů vyberte hello Import tooAzure SQL Data Warehouse odkaz toostart hello Průvodce importem.</span><span class="sxs-lookup"><span data-stu-id="7dcd6-129">From hello DPS main screen, select hello Import tooAzure SQL Data Warehouse link toostart hello import wizard.</span></span>

![][1]

## <a name="step-3-install-hello-data-platform-studio-gateway"></a><span data-ttu-id="7dcd6-130">Krok 3: Instalace hello brána Studio platformy dat</span><span class="sxs-lookup"><span data-stu-id="7dcd6-130">Step 3: Install hello Data Platform Studio Gateway</span></span>
<span data-ttu-id="7dcd6-131">tooconnect tooyour databáze SQL serveru místní, musíte tooinstall hello brány distribučních bodů.</span><span class="sxs-lookup"><span data-stu-id="7dcd6-131">tooconnect tooyour on-premises SQL Server database, you need tooinstall hello DPS Gateway.</span></span> <span data-ttu-id="7dcd6-132">Brána Hello je klientský agent, který poskytuje přístup tooyour místní prostředí, extrahuje hello data a odešle ho tooyour účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="7dcd6-132">hello gateway is a client agent that provides access tooyour on-premises environment, extracts hello data, and uploads it tooyour storage account.</span></span> <span data-ttu-id="7dcd6-133">Vaše data nikdy neprocházejí přes servery společnosti Redgate.</span><span class="sxs-lookup"><span data-stu-id="7dcd6-133">Your data never passes through Redgate’s servers.</span></span> <span data-ttu-id="7dcd6-134">tooinstall hello brány:</span><span class="sxs-lookup"><span data-stu-id="7dcd6-134">tooinstall hello Gateway:</span></span>

1. <span data-ttu-id="7dcd6-135">Klikněte na tlačítko hello **vytvořit bránu** odkaz</span><span class="sxs-lookup"><span data-stu-id="7dcd6-135">Click hello **Create Gateway** link</span></span>
2. <span data-ttu-id="7dcd6-136">Stažení a instalace hello brány pomocí hello zadaný instalační program</span><span class="sxs-lookup"><span data-stu-id="7dcd6-136">Download and install hello Gateway using hello provided installer</span></span>

![][2]

> [!NOTE]
> <span data-ttu-id="7dcd6-137">Hello brány lze nainstalovat z jakéhokoli počítače s databází SQL Server zdrojové toohello přístup k síti.</span><span class="sxs-lookup"><span data-stu-id="7dcd6-137">hello Gateway can be installed on any machine with network access toohello source SQL Server database.</span></span> <span data-ttu-id="7dcd6-138">Přistupuje k databázi systému SQL Server hello pomocí ověřování systému Windows hello přihlašovací údaje aktuálního uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="7dcd6-138">It accesses hello SQL Server database using Windows authentication with hello credentials of hello current user.</span></span>
> 
> 

<span data-ttu-id="7dcd6-139">Po instalaci hello tooConnected změny stavu brány a můžete vybrat další.</span><span class="sxs-lookup"><span data-stu-id="7dcd6-139">Once installed, hello Gateway status changes tooConnected and you can select Next.</span></span>

## <a name="step-4-identify-hello-source-database"></a><span data-ttu-id="7dcd6-140">Krok 4: Určení hello zdrojové databáze</span><span class="sxs-lookup"><span data-stu-id="7dcd6-140">Step 4: Identify hello source database</span></span>
<span data-ttu-id="7dcd6-141">V hello *zadejte název serveru* textovému poli, zadejte název hello hello serveru, který je hostitelem databáze a vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="7dcd6-141">In hello *Enter Server Name* textbox, enter hello name of hello server that hosts your database and select **Next**.</span></span> <span data-ttu-id="7dcd6-142">Potom vyberte z rozevírací nabídky hello, hello požadovaná tooimport data z databáze.</span><span class="sxs-lookup"><span data-stu-id="7dcd6-142">Then, from hello drop-down menu, select hello database you want tooimport data from.</span></span>

![][3]

<span data-ttu-id="7dcd6-143">Distribučních bodů zkontroluje hello vybrané databáze pro tooimport tabulky.</span><span class="sxs-lookup"><span data-stu-id="7dcd6-143">DPS inspects hello selected database for tables tooimport.</span></span> <span data-ttu-id="7dcd6-144">Ve výchozím nastavení distribučních bodů importuje všechny hello tabulky v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="7dcd6-144">By default, DPS imports all hello tables in hello database.</span></span> <span data-ttu-id="7dcd6-145">Můžete vybrat, nebo zrušte výběr tabulek rozšířením hello propojení všech tabulek.</span><span class="sxs-lookup"><span data-stu-id="7dcd6-145">You can select or deselect tables by expanding hello All Tables link.</span></span> <span data-ttu-id="7dcd6-146">Vyberte hello další tlačítko toomove dál.</span><span class="sxs-lookup"><span data-stu-id="7dcd6-146">Select hello Next button toomove forward.</span></span>

## <a name="step-5-choose-a-storage-account-toostage-hello-data"></a><span data-ttu-id="7dcd6-147">Krok 5: Zvolte dat hello toostage účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="7dcd6-147">Step 5: Choose a storage account toostage hello data</span></span>
<span data-ttu-id="7dcd6-148">Distribučních bodů vyzve k zadání umístění toostage hello data.</span><span class="sxs-lookup"><span data-stu-id="7dcd6-148">DPS prompts you for a location toostage hello data.</span></span> <span data-ttu-id="7dcd6-149">Vyberte existující účet úložiště z vašeho předplatného a pak zvolte **Další**.</span><span class="sxs-lookup"><span data-stu-id="7dcd6-149">Choose an existing storage account from your subscription and select **Next**.</span></span>

> [!NOTE]
> <span data-ttu-id="7dcd6-150">Vytvoříte nový kontejner objektů blob v hello vybrali účet úložiště a používat odlišné složku pro každý import distribučních bodů.</span><span class="sxs-lookup"><span data-stu-id="7dcd6-150">DPS will create a new blob container in hello chosen storage account and use a distinct folder for each import.</span></span>
> 
> 

![][4]

## <a name="step-6-select-a-data-warehouse"></a><span data-ttu-id="7dcd6-151">Krok 6: Výběr datového skladu</span><span class="sxs-lookup"><span data-stu-id="7dcd6-151">Step 6: Select a data warehouse</span></span>
<span data-ttu-id="7dcd6-152">Potom vyberte online [Azure SQL Data Warehouse](http://aka.ms/sqldw) tooimport hello data do databáze.</span><span class="sxs-lookup"><span data-stu-id="7dcd6-152">Next, you select an online [Azure SQL Data Warehouse](http://aka.ms/sqldw) database tooimport hello data into.</span></span> <span data-ttu-id="7dcd6-153">Po dokončení výběru vaší databáze, je nutné tooenter hello pověření tooconnect toohello databáze a vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="7dcd6-153">Once you've selected your database, you need tooenter hello credentials tooconnect toohello database and select **Next**.</span></span>

![][5]

> [!NOTE]
> <span data-ttu-id="7dcd6-154">Distribučních bodů sloučí hello zdroje dat tabulky hello datového skladu.</span><span class="sxs-lookup"><span data-stu-id="7dcd6-154">DPS merges hello source data tables into hello data warehouse.</span></span> <span data-ttu-id="7dcd6-155">Pokud název tabulky hello vyžaduje toooverwrite existující tabulky v datovém skladu hello varuje distribučních bodů.</span><span class="sxs-lookup"><span data-stu-id="7dcd6-155">DPS warns you if hello table name requires it toooverwrite existing tables in hello data warehouse.</span></span> <span data-ttu-id="7dcd6-156">Vám může volitelně můžete odstranit všechny existující objekty v datovém skladu hello zaškrtnutím odstranit všechny existující objekty před importem.</span><span class="sxs-lookup"><span data-stu-id="7dcd6-156">You may optionally delete any existing objects in hello data warehouse by ticking Delete all existing objects before import.</span></span>
> 
> 

## <a name="step-7-import-hello-data"></a><span data-ttu-id="7dcd6-157">Krok 7: Import dat hello</span><span class="sxs-lookup"><span data-stu-id="7dcd6-157">Step 7: Import hello data</span></span>
<span data-ttu-id="7dcd6-158">Distribučních bodů, potvrdí, které chcete tooimport hello data.</span><span class="sxs-lookup"><span data-stu-id="7dcd6-158">DPS confirms that you would like tooimport hello data.</span></span> <span data-ttu-id="7dcd6-159">Jednoduše klikněte na tlačítko import dat hello tlačítko hello zahájení importu toobegin.</span><span class="sxs-lookup"><span data-stu-id="7dcd6-159">Simply click hello Start import button toobegin hello data import.</span></span>

![][6]

<span data-ttu-id="7dcd6-160">Distribučních bodů zobrazí vizualizace, který zobrazuje průběh hello extrahování a nahrání dat hello z místní hello systému SQL Server a hello průběh hello import do SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="7dcd6-160">DPS displays a visualization that shows hello progress of extracting and uploading hello data from hello on-premises SQL Server and hello progress of hello import into SQL Data Warehouse.</span></span>

![][7]

<span data-ttu-id="7dcd6-161">Po dokončení importu hello distribučních bodů zobrazuje souhrn importu dat hello a změnit sestavu hello kompatibility opravy, které byly provedeny.</span><span class="sxs-lookup"><span data-stu-id="7dcd6-161">Once hello import is complete, DPS displays a summary of hello data import and a change report of hello compatibility fixes that have been performed.</span></span>

![][8]

## <a name="next-steps"></a><span data-ttu-id="7dcd6-162">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7dcd6-162">Next steps</span></span>
<span data-ttu-id="7dcd6-163">tooexplore svá data do SQL Data Warehouse, spusťte zobrazení:</span><span class="sxs-lookup"><span data-stu-id="7dcd6-163">tooexplore your data within SQL Data Warehouse, start by viewing:</span></span>

* <span data-ttu-id="7dcd6-164">[Dotazování Azure SQL Data Warehouse (Visual Studio)][Query Azure SQL Data Warehouse (Visual Studio)]</span><span class="sxs-lookup"><span data-stu-id="7dcd6-164">[Query Azure SQL Data Warehouse (Visual Studio)][Query Azure SQL Data Warehouse (Visual Studio)]</span></span>
* <span data-ttu-id="7dcd6-165">[Vizualizace dat pomocí Power BI][Visualize data with Power BI]</span><span class="sxs-lookup"><span data-stu-id="7dcd6-165">[Visualize data with Power BI][Visualize data with Power BI]</span></span>

<span data-ttu-id="7dcd6-166">Další informace o společnosti Redgate Data platformy Studio toolearn:</span><span class="sxs-lookup"><span data-stu-id="7dcd6-166">toolearn more about Redgate’s Data Platform Studio:</span></span>

* [<span data-ttu-id="7dcd6-167">Navštěvovat domovskou stránku hello distribučních bodů</span><span class="sxs-lookup"><span data-stu-id="7dcd6-167">Visit hello DPS homepage</span></span>](http://www.dataplatformstudio.com/)
* [<span data-ttu-id="7dcd6-168">Ukázka DPS na Channel 9</span><span class="sxs-lookup"><span data-stu-id="7dcd6-168">Watch a demo of DPS on Channel9</span></span>](https://channel9.msdn.com/Blogs/cloud-with-a-silver-lining/Loading-data-into-Azure-SQL-Datawarehouse-with-Redgate-Data-Platform-Studio)

<span data-ttu-id="7dcd6-169">Vaše data v SQL Data Warehouse najdete přehled jiné způsoby toomigrate a zatížení:</span><span class="sxs-lookup"><span data-stu-id="7dcd6-169">For an overview of other ways toomigrate and load your data in SQL Data Warehouse see:</span></span>

* <span data-ttu-id="7dcd6-170">[Migrace vašeho řešení tooSQL datového skladu][Migrate your solution tooSQL Data Warehouse]</span><span class="sxs-lookup"><span data-stu-id="7dcd6-170">[Migrate your solution tooSQL Data Warehouse][Migrate your solution tooSQL Data Warehouse]</span></span>
* [<span data-ttu-id="7dcd6-171">Načtení dat do Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="7dcd6-171">Load data into Azure SQL Data Warehouse</span></span>](sql-data-warehouse-overview-load.md)

<span data-ttu-id="7dcd6-172">Další tipy pro vývoj, najdete v části hello [přehled vývoje SQL Data Warehouse](sql-data-warehouse-overview-develop.md).</span><span class="sxs-lookup"><span data-stu-id="7dcd6-172">For more development tips, see hello [SQL Data Warehouse development overview](sql-data-warehouse-overview-develop.md).</span></span>

<!--Image references-->
[1]: media/sql-data-warehouse-redgate/2016-10-05_15-59-56.png
[2]: media/sql-data-warehouse-redgate/2016-10-05_11-16-07.png
[3]: media/sql-data-warehouse-redgate/2016-10-05_11-17-46.png
[4]: media/sql-data-warehouse-redgate/2016-10-05_11-20-41.png
[5]: media/sql-data-warehouse-redgate/2016-10-05_11-31-24.png
[6]: media/sql-data-warehouse-redgate/2016-10-05_11-32-20.png
[7]: media/sql-data-warehouse-redgate/2016-10-05_11-49-53.png
[8]: media/sql-data-warehouse-redgate/2016-10-05_12-57-10.png

<!--Article references-->
[Query Azure SQL Data Warehouse (Visual Studio)]: ./sql-data-warehouse-query-visual-studio.md
[Visualize data with Power BI]: ./sql-data-warehouse-get-started-visualize-with-power-bi.md
[Migrate your solution tooSQL Data Warehouse]: ./sql-data-warehouse-overview-migrate.md
[Load data into Azure SQL Data Warehouse]: ./sql-data-warehouse-overview-load.md
[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md
