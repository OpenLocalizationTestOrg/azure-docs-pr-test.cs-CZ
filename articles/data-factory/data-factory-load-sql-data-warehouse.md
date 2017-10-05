---
title: "Načtení terabajtů dat do SQL Data Warehouse | Microsoft Docs"
description: "Ukazuje, jak 1 TB dat je možné načíst do Azure SQL Data Warehouse pomocí Azure Data Factory v části 15 minut"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: a6c133c0-ced2-463c-86f0-a07b00c9e37f
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jingwang
ms.openlocfilehash: c29f1f01b660c4eb780e178a68036327fafa9ba6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="load-1-tb-into-azure-sql-data-warehouse-under-15-minutes-with-data-factory"></a><span data-ttu-id="36c66-103">Načtení 1 TB do Azure SQL Data Warehouse pomocí služby Data Factory v části 15 minut</span><span class="sxs-lookup"><span data-stu-id="36c66-103">Load 1 TB into Azure SQL Data Warehouse under 15 minutes with Data Factory</span></span>
<span data-ttu-id="36c66-104">[Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) je databáze založená na cloudu, škálování dokáže zpracovávat ohromné objemy dat, relačních i nerelačních.</span><span class="sxs-lookup"><span data-stu-id="36c66-104">[Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) is a cloud-based, scale-out database capable of processing massive volumes of data, both relational and non-relational.</span></span>  <span data-ttu-id="36c66-105">Postavená na architektuře massively parallel processing (MPP), SQL Data Warehouse je optimalizovaná pro podnikové datového skladu úlohy.</span><span class="sxs-lookup"><span data-stu-id="36c66-105">Built on massively parallel processing (MPP) architecture, SQL Data Warehouse is optimized for enterprise data warehouse workloads.</span></span>  <span data-ttu-id="36c66-106">Pružnost cloudu nabízí flexibilitu škálovat úložiště a výpočetní nezávisle.</span><span class="sxs-lookup"><span data-stu-id="36c66-106">It offers cloud elasticity with the flexibility to scale storage and compute independently.</span></span>

<span data-ttu-id="36c66-107">Začínáme s Azure SQL Data Warehouse je teď jednodušší než kdy použití **Azure Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="36c66-107">Getting started with Azure SQL Data Warehouse is now easier than ever using **Azure Data Factory**.</span></span>  <span data-ttu-id="36c66-108">Azure Data Factory je plně spravovaná Cloudová datová integrační služby, která slouží k naplnění SQL Data Warehouse s daty z vašeho stávajícího systému a ukládání je hodně času při vyhodnocení SQL Data Warehouse a sestavování analytické údaje řešení.</span><span class="sxs-lookup"><span data-stu-id="36c66-108">Azure Data Factory is a fully managed cloud-based data integration service, which can be used to populate a SQL Data Warehouse with the data from your existing system, and saving you valuable time while evaluating SQL Data Warehouse and building your analytics solutions.</span></span> <span data-ttu-id="36c66-109">Tady jsou klíčové výhody načítání dat do Azure SQL Data Warehouse pomocí Azure Data Factory:</span><span class="sxs-lookup"><span data-stu-id="36c66-109">Here are the key benefits of loading data into Azure SQL Data Warehouse using Azure Data Factory:</span></span>

* <span data-ttu-id="36c66-110">**Snadno nastavit**: krok 5 intuitivní průvodce bez potřeby skriptování.</span><span class="sxs-lookup"><span data-stu-id="36c66-110">**Easy to set up**: 5-step intuitive wizard with no scripting required.</span></span>
* <span data-ttu-id="36c66-111">**Podpora úložiště bohaté dat**: integrovanou podporu pro bohatou sadu místní a Cloudová datová úložiště.</span><span class="sxs-lookup"><span data-stu-id="36c66-111">**Rich data store support**: built-in support for a rich set of on-premises and cloud-based data stores.</span></span>
* <span data-ttu-id="36c66-112">**Zabezpečení a dodržování**: data se přenáší prostřednictvím protokolu HTTPS nebo ExpressRoute a přítomnost globální služby zajišťuje vaše data nikdy neopustí zeměpisné hranic</span><span class="sxs-lookup"><span data-stu-id="36c66-112">**Secure and compliant**: data is transferred over HTTPS or ExpressRoute, and global service presence ensures your data never leaves the geographical boundary</span></span>
* <span data-ttu-id="36c66-113">**Bezkonkurenční výkonu pomocí funkce PolyBase** – pomocí Polybase je nejúčinnější způsob, jak přesunout data do Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="36c66-113">**Unparalleled performance by using PolyBase** – Using Polybase is the most efficient way to move data into Azure SQL Data Warehouse.</span></span> <span data-ttu-id="36c66-114">Používá funkci pracovní objekt blob, můžete dosáhnout vysokého zatížení rychlosti ze všech typů úložišť dat kromě úložiště objektů Blob v Azure, což Polybase podporuje ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="36c66-114">Using the staging blob feature, you can achieve high load speeds from all types of data stores besides Azure Blob storage, which the Polybase supports by default.</span></span>

<span data-ttu-id="36c66-115">Tento článek ukazuje, jak používat Průvodce kopírováním služby Data Factory k načtení 1 TB dat z Azure Blob Storage do Azure SQL Data Warehouse v části 15 minut, při propustnost přes 1,2 GB/s.</span><span class="sxs-lookup"><span data-stu-id="36c66-115">This article shows you how to use Data Factory Copy Wizard to load 1-TB data from Azure Blob Storage into Azure SQL Data Warehouse in under 15 minutes, at over 1.2 GBps throughput.</span></span>

<span data-ttu-id="36c66-116">Tento článek obsahuje podrobné pokyny pro přesun dat do Azure SQL Data Warehouse pomocí Průvodce kopírováním.</span><span class="sxs-lookup"><span data-stu-id="36c66-116">This article provides step-by-step instructions for moving data into Azure SQL Data Warehouse by using the Copy Wizard.</span></span>

> [!NOTE]
>  <span data-ttu-id="36c66-117">Obecné informace o možnostech objektu pro vytváření dat při přesunu dat do/z Azure SQL Data Warehouse najdete v tématu [přesun dat do a z Azure SQL Data Warehouse pomocí Azure Data Factory](data-factory-azure-sql-data-warehouse-connector.md) článku.</span><span class="sxs-lookup"><span data-stu-id="36c66-117">For general information about capabilities of Data Factory in moving data to/from Azure SQL Data Warehouse, see [Move data to and from Azure SQL Data Warehouse using Azure Data Factory](data-factory-azure-sql-data-warehouse-connector.md) article.</span></span>
>
> <span data-ttu-id="36c66-118">Můžete také vytvořit pomocí portálu Azure, Visual Studio, prostředí PowerShell, kanály atd. V tématu [kurz: kopírování dat z objektu Blob Azure do Azure SQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) rychlé návod s podrobnými pokyny pro používání aktivity kopírování v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="36c66-118">You can also build pipelines using Azure portal, Visual Studio, PowerShell, etc. See [Tutorial: Copy data from Azure Blob to Azure SQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for a quick walkthrough with step-by-step instructions for using the Copy Activity in Azure Data Factory.</span></span>  
>
>

## <a name="prerequisites"></a><span data-ttu-id="36c66-119">Požadavky</span><span class="sxs-lookup"><span data-stu-id="36c66-119">Prerequisites</span></span>
* <span data-ttu-id="36c66-120">Azure Blob Storage: Tento experiment používá Azure Blob úložiště (GRS) pro ukládání TPC-H testování datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="36c66-120">Azure Blob Storage: this experiment uses Azure Blob Storage (GRS) for storing TPC-H testing dataset.</span></span>  <span data-ttu-id="36c66-121">Pokud nemáte účet úložiště Azure, přečtěte si [postup vytvoření účtu úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account).</span><span class="sxs-lookup"><span data-stu-id="36c66-121">If you do not have an Azure storage account, learn [how to create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account).</span></span>
* <span data-ttu-id="36c66-122">[TPC-H](http://www.tpc.org/tpch/) data: budeme používat TPC-H jako testování datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="36c66-122">[TPC-H](http://www.tpc.org/tpch/) data: we are going to use TPC-H as the testing dataset.</span></span>  <span data-ttu-id="36c66-123">K tomu, budete muset použít `dbgen` z TPC-H nástrojů, který umožňuje generovat datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="36c66-123">To do that, you need to use `dbgen` from TPC-H toolkit, which helps you generate the dataset.</span></span>  <span data-ttu-id="36c66-124">Můžete buď stáhnout zdrojového kódu pro `dbgen` z [TPC nástroje](http://www.tpc.org/tpc_documents_current_versions/current_specifications.asp) a zkompilovat sami nebo stáhnout kompilované binárního souboru z [Githubu](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TPCHTools).</span><span class="sxs-lookup"><span data-stu-id="36c66-124">You can either download source code for `dbgen` from [TPC Tools](http://www.tpc.org/tpc_documents_current_versions/current_specifications.asp) and compile it yourself, or download the compiled binary from [GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TPCHTools).</span></span>  <span data-ttu-id="36c66-125">Spusťte dbgen.exe s následující příkazy ke generování plochého souboru 1 TB pro `lineitem` tabulky šíření mezi 10 souborů:</span><span class="sxs-lookup"><span data-stu-id="36c66-125">Run dbgen.exe with the following commands to generate 1 TB flat file for `lineitem` table spread across 10 files:</span></span>

  * `Dbgen -s 1000 -S **1** -C 10 -T L -v`
  * `Dbgen -s 1000 -S **2** -C 10 -T L -v`
  * <span data-ttu-id="36c66-126">…</span><span class="sxs-lookup"><span data-stu-id="36c66-126">…</span></span>
  * `Dbgen -s 1000 -S **10** -C 10 -T L -v`

    <span data-ttu-id="36c66-127">Nyní zkopírujte soubory do objektu Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="36c66-127">Now copy the generated files to Azure Blob.</span></span>  <span data-ttu-id="36c66-128">Odkazovat na [přesun dat do a ze v místním systému souborů pomocí Azure Data Factory](data-factory-onprem-file-system-connector.md) jak to udělat pomocí kopírování ADF.</span><span class="sxs-lookup"><span data-stu-id="36c66-128">Refer to [Move data to and from an on-premises file system by using Azure Data Factory](data-factory-onprem-file-system-connector.md) for how to do that using ADF Copy.</span></span>    
* <span data-ttu-id="36c66-129">Azure SQL Data Warehouse: tohoto experimentu načte data do Azure SQL Data Warehouse vytvořené pomocí 6000 Dwu</span><span class="sxs-lookup"><span data-stu-id="36c66-129">Azure SQL Data Warehouse: this experiment loads data into Azure SQL Data Warehouse created with 6,000 DWUs</span></span>

    <span data-ttu-id="36c66-130">Odkazovat na [vytvoření Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md) podrobné pokyny o tom, jak vytvořit databázi SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="36c66-130">Refer to [Create an Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md) for detailed instructions on how to create a SQL Data Warehouse database.</span></span>  <span data-ttu-id="36c66-131">Chcete-li získat nejlepšího výkonu dosáhnete možné zatížení do SQL Data Warehouse pomocí Polybase, jsme zvolte maximální počet jednotek datového skladu (Dwu) povolen v nastavení výkonu, který je 6000 Dwu.</span><span class="sxs-lookup"><span data-stu-id="36c66-131">To get the best possible load performance into SQL Data Warehouse using Polybase, we choose maximum number of Data Warehouse Units (DWUs) allowed in the Performance setting, which is 6,000 DWUs.</span></span>

  > [!NOTE]
  > <span data-ttu-id="36c66-132">Při načítání z Azure Blob, načítání výkonu dat je přímo úměrná počtu jednotek Dwu, které nakonfigurujete v SQL Data Warehouse:</span><span class="sxs-lookup"><span data-stu-id="36c66-132">When loading from Azure Blob, the data loading performance is directly proportional to the number of DWUs you configure on the SQL Data Warehouse:</span></span>
  >
  > <span data-ttu-id="36c66-133">Načtení 1 TB do 1000 DWU SQL Data Warehouse přebírá načítání 1 TB 87 minut (propustnost ~ 200 MB/s) do 2 000 DWU SQL Data Warehouse trvá 46 minut (propustnost ~ 380 MB/s) načítání 1 TB do 6000 DWU SQL Data Warehouse přebírá 14 minut (propustnost ~1.2 GB/s)</span><span class="sxs-lookup"><span data-stu-id="36c66-133">Loading 1 TB into 1,000 DWU SQL Data Warehouse takes 87 minutes (~200 MBps throughput) Loading 1 TB into 2,000 DWU SQL Data Warehouse takes 46 minutes (~380 MBps throughput) Loading 1 TB into 6,000 DWU SQL Data Warehouse takes 14 minutes (~1.2 GBps throughput)</span></span>
  >
  >

    <span data-ttu-id="36c66-134">Chcete-li vytvořit SQL Data Warehouse s 6000 Dwu, posuňte jezdec výkonu úplně napravo:</span><span class="sxs-lookup"><span data-stu-id="36c66-134">To create a SQL Data Warehouse with 6,000 DWUs, move the Performance slider all the way to the right:</span></span>

    ![Výkon posuvníku](media/data-factory-load-sql-data-warehouse/performance-slider.png)

    <span data-ttu-id="36c66-136">Pro existující databázi, který není konfigurován s 6000 Dwu můžete postupně škálovat ji pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="36c66-136">For an existing database that is not configured with 6,000 DWUs, you can scale it up using Azure portal.</span></span>  <span data-ttu-id="36c66-137">Přejděte do databáze na portálu Azure a je **škálování** v tlačítko **přehled** panelu vidět na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="36c66-137">Navigate to the database in Azure portal, and there is a **Scale** button in the **Overview** panel shown in the following image:</span></span>

    ![Stupnice – tlačítko](media/data-factory-load-sql-data-warehouse/scale-button.png)    

    <span data-ttu-id="36c66-139">Klikněte **škálování** otevřete následující panely, přesuňte jezdec na maximální hodnotu, a klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="36c66-139">Click the **Scale** button to open the following panel, move the slider to the maximum value, and click **Save** button.</span></span>

    ![Dialogové okno škálování](media/data-factory-load-sql-data-warehouse/scale-dialog.png)

    <span data-ttu-id="36c66-141">Tento experiment načte data do Azure SQL Data Warehouse pomocí `xlargerc` Třída prostředků.</span><span class="sxs-lookup"><span data-stu-id="36c66-141">This experiment loads data into Azure SQL Data Warehouse using `xlargerc` resource class.</span></span>

    <span data-ttu-id="36c66-142">K dosažení nejlepší možné propustnost, je nutné provést pomocí SQL Data Warehouse uživatel patřící do kopie `xlargerc` Třída prostředků.</span><span class="sxs-lookup"><span data-stu-id="36c66-142">To achieve best possible throughput, copy needs to be performed using a SQL Data Warehouse user belonging to `xlargerc` resource class.</span></span>  <span data-ttu-id="36c66-143">Zjistěte, jak to provést pomocí následujících [změnit v příkladu třída prostředků uživatele](../sql-data-warehouse/sql-data-warehouse-develop-concurrency.md#changing-user-resource-class-example).</span><span class="sxs-lookup"><span data-stu-id="36c66-143">Learn how to do that by following [Change a user resource class example](../sql-data-warehouse/sql-data-warehouse-develop-concurrency.md#changing-user-resource-class-example).</span></span>  
* <span data-ttu-id="36c66-144">Vytvoření schématu cílové tabulky v databázi Azure SQL Data Warehouse spuštěním následujícího příkazu DDL:</span><span class="sxs-lookup"><span data-stu-id="36c66-144">Create destination table schema in Azure SQL Data Warehouse database, by running the following DDL statement:</span></span>

    ```SQL  
    CREATE TABLE [dbo].[lineitem]
    (
        [L_ORDERKEY] [bigint] NOT NULL,
        [L_PARTKEY] [bigint] NOT NULL,
        [L_SUPPKEY] [bigint] NOT NULL,
        [L_LINENUMBER] [int] NOT NULL,
        [L_QUANTITY] [decimal](15, 2) NULL,
        [L_EXTENDEDPRICE] [decimal](15, 2) NULL,
        [L_DISCOUNT] [decimal](15, 2) NULL,
        [L_TAX] [decimal](15, 2) NULL,
        [L_RETURNFLAG] [char](1) NULL,
        [L_LINESTATUS] [char](1) NULL,
        [L_SHIPDATE] [date] NULL,
        [L_COMMITDATE] [date] NULL,
        [L_RECEIPTDATE] [date] NULL,
        [L_SHIPINSTRUCT] [char](25) NULL,
        [L_SHIPMODE] [char](10) NULL,
        [L_COMMENT] [varchar](44) NULL
    )
    WITH
    (
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    ```
<span data-ttu-id="36c66-145">O postupech, pomocí požadovaných součástí byla dokončena jsme nyní připraveni ke konfiguraci aktivitou kopírování pomocí Průvodce kopírováním.</span><span class="sxs-lookup"><span data-stu-id="36c66-145">With the prerequisite steps completed, we are now ready to configure the copy activity using the Copy Wizard.</span></span>

## <a name="launch-copy-wizard"></a><span data-ttu-id="36c66-146">Spuštění průvodce kopírováním</span><span class="sxs-lookup"><span data-stu-id="36c66-146">Launch Copy Wizard</span></span>
1. <span data-ttu-id="36c66-147">Přihlaste se k portálu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="36c66-147">Log in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="36c66-148">Klikněte na tlačítko **+ nový** v levém horním rohu klikněte na **Intelligence + analýzy**a klikněte na tlačítko **Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="36c66-148">Click **+ NEW** from the top-left corner, click **Intelligence + analytics**, and click **Data Factory**.</span></span>
3. <span data-ttu-id="36c66-149">V okně **Nový objekt pro vytváření dat**:</span><span class="sxs-lookup"><span data-stu-id="36c66-149">In the **New data factory** blade:</span></span>

   1. <span data-ttu-id="36c66-150">Zadejte **LoadIntoSQLDWDataFactory** pro **název**.</span><span class="sxs-lookup"><span data-stu-id="36c66-150">Enter **LoadIntoSQLDWDataFactory** for the **name**.</span></span>
       <span data-ttu-id="36c66-151">Název objektu pro vytváření dat Azure musí být globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="36c66-151">The name of the Azure data factory must be globally unique.</span></span> <span data-ttu-id="36c66-152">Pokud se zobrazí chyba: **název objektu pro vytváření dat "LoadIntoSQLDWDataFactory" není k dispozici**, změňte název objektu pro vytváření dat (například yournameLoadIntoSQLDWDataFactory) a zkuste to znovu.</span><span class="sxs-lookup"><span data-stu-id="36c66-152">If you receive the error: **Data factory name “LoadIntoSQLDWDataFactory” is not available**, change the name of the data factory (for example, yournameLoadIntoSQLDWDataFactory) and try creating again.</span></span> <span data-ttu-id="36c66-153">V tématu [Objekty pro vytváření dat – pravidla pojmenování](data-factory-naming-rules.md) najdete pravidla pojmenování artefaktů služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="36c66-153">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>  
   2. <span data-ttu-id="36c66-154">Vyberte své **předplatné** Azure.</span><span class="sxs-lookup"><span data-stu-id="36c66-154">Select your Azure **subscription**.</span></span>
   3. <span data-ttu-id="36c66-155">Pro skupinu prostředků proveďte jeden z následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="36c66-155">For Resource Group, do one of the following steps:</span></span>
      1. <span data-ttu-id="36c66-156">Vyberte možnost **Použít existující** a vyberte existující skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="36c66-156">Select **Use existing** to select an existing resource group.</span></span>
      2. <span data-ttu-id="36c66-157">Vyberte možnost **Vytvořit nový** a zadejte název pro skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="36c66-157">Select **Create new** to enter a name for a resource group.</span></span>
   4. <span data-ttu-id="36c66-158">Vyberte **umístění** pro příslušný objekt pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="36c66-158">Select a **location** for the data factory.</span></span>
   5. <span data-ttu-id="36c66-159">Zaškrtněte políčko **Připnout na řídicí panel** v dolní části okna.</span><span class="sxs-lookup"><span data-stu-id="36c66-159">Select **Pin to dashboard** check box at the bottom of the blade.</span></span>  
   6. <span data-ttu-id="36c66-160">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="36c66-160">Click **Create**.</span></span>
4. <span data-ttu-id="36c66-161">Po vytvoření se zobrazí okno **Objekt pro vytváření dat**, jak je znázorněno na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="36c66-161">After the creation is complete, you see the **Data Factory** blade as shown in the following image:</span></span>

   ![Domovská stránka objektu pro vytváření dat](media/data-factory-load-sql-data-warehouse/data-factory-home-page-copy-data.png)
5. <span data-ttu-id="36c66-163">Na domovské stránce objektu pro vytváření dat klikněte na dlaždici **Kopírovat data**. Spustí se **průvodce kopírováním**.</span><span class="sxs-lookup"><span data-stu-id="36c66-163">On the Data Factory home page, click the **Copy data** tile to launch **Copy Wizard**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="36c66-164">Pokud zjistíte, že se webový prohlížeč zasekl ve fázi „Autorizace…“, zakažte / zrušte zaškrtnutí políčka **Block third party cookies and site data** (Blokovat data souborů cookie a webů třetích stran) nebo nechte toto nastavení povolené a vytvořte výjimku pro **login.microsoftonline.com**. Potom zkuste průvodce znovu spustit.</span><span class="sxs-lookup"><span data-stu-id="36c66-164">If you see that the web browser is stuck at "Authorizing...", disable/uncheck **Block third party cookies and site data** setting (or) keep it enabled and create an exception for **login.microsoftonline.com** and then try launching the wizard again.</span></span>
   >
   >

## <a name="step-1-configure-data-loading-schedule"></a><span data-ttu-id="36c66-165">Krok 1: Konfigurace plánu pro načítání dat</span><span class="sxs-lookup"><span data-stu-id="36c66-165">Step 1: Configure data loading schedule</span></span>
<span data-ttu-id="36c66-166">Prvním krokem je konfigurace dat načítání plánu.</span><span class="sxs-lookup"><span data-stu-id="36c66-166">The first step is to configure the data loading schedule.</span></span>  

<span data-ttu-id="36c66-167">Na stránce **Vlastnosti**:</span><span class="sxs-lookup"><span data-stu-id="36c66-167">In the **Properties** page:</span></span>

1. <span data-ttu-id="36c66-168">Zadejte **CopyFromBlobToAzureSqlDataWarehouse** pro **název úlohy**</span><span class="sxs-lookup"><span data-stu-id="36c66-168">Enter **CopyFromBlobToAzureSqlDataWarehouse** for **Task name**</span></span>
2. <span data-ttu-id="36c66-169">Vyberte **spustit jednou nyní** možnost.</span><span class="sxs-lookup"><span data-stu-id="36c66-169">Select **Run once now** option.</span></span>   
3. <span data-ttu-id="36c66-170">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="36c66-170">Click **Next**.</span></span>  

    ![Průvodce kopírováním – stránky vlastností](media/data-factory-load-sql-data-warehouse/copy-wizard-properties-page.png)

## <a name="step-2-configure-source"></a><span data-ttu-id="36c66-172">Krok 2: Konfigurace zdroje</span><span class="sxs-lookup"><span data-stu-id="36c66-172">Step 2: Configure source</span></span>
<span data-ttu-id="36c66-173">Tato část popisuje postup konfigurace zdroj: Azure Blob obsahující TPC 1 TB-H položky na řádku soubory.</span><span class="sxs-lookup"><span data-stu-id="36c66-173">This section shows you the steps to configure the source: Azure Blob containing the 1-TB TPC-H line item files.</span></span>

1. <span data-ttu-id="36c66-174">Vyberte **Azure Blob Storage** jako úložiště dat a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="36c66-174">Select the **Azure Blob Storage** as the data store and click **Next**.</span></span>

    ![Průvodce kopírováním - vyberte zdroj stránky](media/data-factory-load-sql-data-warehouse/select-source-connection.png)

2. <span data-ttu-id="36c66-176">Zadejte informace o připojení pro účet úložiště objektů Blob v Azure a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="36c66-176">Fill in the connection information for the Azure Blob storage account, and click **Next**.</span></span>

    ![Průvodce kopírováním - informace o připojení zdroje](media/data-factory-load-sql-data-warehouse/source-connection-info.png)

3. <span data-ttu-id="36c66-178">Vyberte **složky** obsahující TPC-H řádek souborů položek a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="36c66-178">Choose the **folder** containing the TPC-H line item files and click **Next**.</span></span>

    ![Průvodce kopírováním – vyberte vstupní složky](media/data-factory-load-sql-data-warehouse/select-input-folder.png)

4. <span data-ttu-id="36c66-180">Po kliknutí na tlačítko **Další**, nastavení formátu souboru se rozpoznávají automaticky.</span><span class="sxs-lookup"><span data-stu-id="36c66-180">Upon clicking **Next**, the file format settings are detected automatically.</span></span>  <span data-ttu-id="36c66-181">Zkontrolujte, zkontrolujte, zda je tento sloupec oddělovač ' | 'místo výchozí čárkou','.</span><span class="sxs-lookup"><span data-stu-id="36c66-181">Check to make sure that column delimiter is ‘|’ instead of the default comma ‘,’.</span></span>  <span data-ttu-id="36c66-182">Klikněte na tlačítko **Další** po mít zobrazení náhledu data.</span><span class="sxs-lookup"><span data-stu-id="36c66-182">Click **Next** after you have previewed the data.</span></span>

    ![Průvodce kopírováním – nastavení formátu souboru](media/data-factory-load-sql-data-warehouse/file-format-settings.png)

## <a name="step-3-configure-destination"></a><span data-ttu-id="36c66-184">Krok 3: Konfigurace cílového</span><span class="sxs-lookup"><span data-stu-id="36c66-184">Step 3: Configure destination</span></span>
<span data-ttu-id="36c66-185">V této části se dozvíte, jak nakonfigurovat cíl: `lineitem` tabulky v databázi Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="36c66-185">This section shows you how to configure the destination: `lineitem` table in the Azure SQL Data Warehouse database.</span></span>

1. <span data-ttu-id="36c66-186">Zvolte **Azure SQL Data Warehouse** jako cíl uložení a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="36c66-186">Choose **Azure SQL Data Warehouse** as the destination store and click **Next**.</span></span>

    ![Průvodce kopírováním - vyberte cílového úložiště dat](media/data-factory-load-sql-data-warehouse/select-destination-data-store.png)

2. <span data-ttu-id="36c66-188">Zadejte informace o připojení pro Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="36c66-188">Fill in the connection information for Azure SQL Data Warehouse.</span></span>  <span data-ttu-id="36c66-189">Je nutné zadat uživatele, který je členem role `xlargerc` (najdete v článku **požadavky** části Podrobné pokyny) a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="36c66-189">Make sure you specify the user that is a member of the role `xlargerc` (see the **prerequisites** section for detailed instructions), and click **Next**.</span></span>

    ![Průvodce kopírováním - informace o cílovém připojení](media/data-factory-load-sql-data-warehouse/destination-connection-info.png)

3. <span data-ttu-id="36c66-191">Vyberte cílové tabulky a klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="36c66-191">Choose the destination table and click **Next**.</span></span>

    ![Zkopírujte - tabulky mapování stránku průvodce](media/data-factory-load-sql-data-warehouse/table-mapping-page.png)

4. <span data-ttu-id="36c66-193">Schéma mapování stránce, nechte nezaškrtnuté možnost "Použijí mapování sloupce" a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="36c66-193">In Schema mapping page, leave "Apply column mapping" option unchecked and click **Next**.</span></span>

## <a name="step-4-performance-settings"></a><span data-ttu-id="36c66-194">Krok 4: Nastavení výkonu</span><span class="sxs-lookup"><span data-stu-id="36c66-194">Step 4: Performance settings</span></span>

<span data-ttu-id="36c66-195">**Povolit polybase** je ve výchozím nastavení zaškrtnuto.</span><span class="sxs-lookup"><span data-stu-id="36c66-195">**Allow polybase** is checked by default.</span></span>  <span data-ttu-id="36c66-196">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="36c66-196">Click **Next**.</span></span>

![Zkopírujte – schéma mapování stránku průvodce](media/data-factory-load-sql-data-warehouse/performance-settings-page.png)

## <a name="step-5-deploy-and-monitor-load-results"></a><span data-ttu-id="36c66-198">Krok 5: Nasazení a monitorování zatížení výsledky</span><span class="sxs-lookup"><span data-stu-id="36c66-198">Step 5: Deploy and monitor load results</span></span>
1. <span data-ttu-id="36c66-199">Klikněte na tlačítko **Dokončit** tlačítko pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="36c66-199">Click **Finish** button to deploy.</span></span>

    ![Průvodce kopírováním - souhrnná stránka](media/data-factory-load-sql-data-warehouse/summary-page.png)

2. <span data-ttu-id="36c66-201">Po dokončení nasazení klikněte na tlačítko `Click here to monitor copy pipeline` monitorování kopie spustit průběh.</span><span class="sxs-lookup"><span data-stu-id="36c66-201">After the deployment is complete, click `Click here to monitor copy pipeline` to monitor the copy run progress.</span></span> <span data-ttu-id="36c66-202">Vyberte kanál kopírování jste vytvořili v **aktivity Windows** seznamu.</span><span class="sxs-lookup"><span data-stu-id="36c66-202">Select the copy pipeline you created in the **Activity Windows** list.</span></span>

    ![Průvodce kopírováním - souhrnná stránka](media/data-factory-load-sql-data-warehouse/select-pipeline-monitor-manage-app.png)

    <span data-ttu-id="36c66-204">Můžete zobrazit podrobnosti o spuštění kopie **aktivity okno Průzkumníka** v pravém panelu, včetně datový svazek číst ze zdroje a zapsána do cílového, doba trvání a průměrná propustnost pro spuštění.</span><span class="sxs-lookup"><span data-stu-id="36c66-204">You can view the copy run details in the **Activity Window Explorer** in the right panel, including the data volume read from source and written into destination, duration, and the average throughput for the run.</span></span>

    <span data-ttu-id="36c66-205">Jak je vidět na následujícím snímku obrazovky, kopírování 1 TB z Azure Blob Storage do SQL Data Warehouse trvalo 14 minut, efektivně dosahování 1.22 GB/s propustnosti!</span><span class="sxs-lookup"><span data-stu-id="36c66-205">As you can see from the following screen shot, copying 1 TB from Azure Blob Storage into SQL Data Warehouse took 14 minutes, effectively achieving 1.22 GBps throughput!</span></span>

    ![Průvodce kopírováním – dialogové okno úspěšně vytvořila.](media/data-factory-load-sql-data-warehouse/succeeded-info.png)

## <a name="best-practices"></a><span data-ttu-id="36c66-207">Osvědčené postupy</span><span class="sxs-lookup"><span data-stu-id="36c66-207">Best practices</span></span>
<span data-ttu-id="36c66-208">Tady je několik osvědčených postupů pro spuštění databáze Azure SQL Data Warehouse:</span><span class="sxs-lookup"><span data-stu-id="36c66-208">Here are a few best practices for running your Azure SQL Data Warehouse database:</span></span>

* <span data-ttu-id="36c66-209">Při načítání do CLUSTEROVANÝ INDEX COLUMNSTORE, použijte větší Třída prostředků.</span><span class="sxs-lookup"><span data-stu-id="36c66-209">Use a larger resource class when loading into a CLUSTERED COLUMNSTORE INDEX.</span></span>
* <span data-ttu-id="36c66-210">Pro efektivnější spojení zvažte použití algoritmu hash distribuce podle vyberte sloupce místo výchozí round robin distribuční.</span><span class="sxs-lookup"><span data-stu-id="36c66-210">For more efficient joins, consider using hash distribution by a select column instead of default round robin distribution.</span></span>
* <span data-ttu-id="36c66-211">Vyšší rychlost zatížení zvažte použití haldy pro přechodný data.</span><span class="sxs-lookup"><span data-stu-id="36c66-211">For faster load speeds, consider using heap for transient data.</span></span>
* <span data-ttu-id="36c66-212">Vytvoření statistiky po dokončení načítání Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="36c66-212">Create statistics after you finish loading Azure SQL Data Warehouse.</span></span>

<span data-ttu-id="36c66-213">V tématu [osvědčené postupy pro Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-best-practices.md) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="36c66-213">See [Best practices for Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-best-practices.md) for details.</span></span>

## <a name="next-steps"></a><span data-ttu-id="36c66-214">Další kroky</span><span class="sxs-lookup"><span data-stu-id="36c66-214">Next steps</span></span>
* <span data-ttu-id="36c66-215">[Průvodce kopírováním služby Data Factory](data-factory-copy-wizard.md) – Tento článek obsahuje podrobné informace o Průvodci kopírováním.</span><span class="sxs-lookup"><span data-stu-id="36c66-215">[Data Factory Copy Wizard](data-factory-copy-wizard.md) - This article provides details about the Copy Wizard.</span></span>
* <span data-ttu-id="36c66-216">[Zkopírujte aktivity výkonu a vyladění průvodce](data-factory-copy-activity-performance.md) – Tento článek obsahuje referenční měření výkonu a vyladění průvodce.</span><span class="sxs-lookup"><span data-stu-id="36c66-216">[Copy Activity performance and tuning guide](data-factory-copy-activity-performance.md) - This article contains the reference performance measurements and tuning guide.</span></span>
